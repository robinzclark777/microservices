

# Scenario

This gist applies when the a locally installed `Kubernetes` cluster isn&rsquo;t able to
see locally built `Docker` images. It allows users to push images from the local
`Docker` daemon into the `Kubernetes` cluster. These instructions were created for
`k3s`. For `Docker Desktop` this shouldn&rsquo;t be required. For `minikube` it may not be
required. Other `Kubernetes` flavors will differ in some specifics.

This process supersedes the `docker save/k3s ctr import` process that&rsquo;s documented
elsewhere.


# Process


## Tell k3s about the registry-to-be

Copy this file to `/etc/rancher/k3s/registries.yaml`:

    mirrors:
      registry.local:
        endpoint:
          - "http://registry.local"


## Create the `Kubernetes` artifacts file

Save this file as `local-docker-registry.yaml`:

    ---
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: docker-registry-ingress
      namespace: docker-registry
      annotations:
        kubernetes.io/ingress.class: "traefik"
    spec:
      rules:
      - host: registry.local
        http:
          paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: docker-registry-service
                port:
                  number: 5000
    
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: docker-registry-service
      namespace: docker-registry
      labels:
        run: docker-registry
    spec:
      selector:
        app: docker-registry
      ports:
        - protocol: TCP
          port: 5000
    
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: docker-registry
      namespace: docker-registry
      labels:
        app: docker-registry
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: docker-registry
      template:
        metadata:
          labels:
            app: docker-registry
        spec:
          containers:
          - name: docker-registry
            image: registry
            ports:
            - containerPort: 5000
              protocol: TCP
            volumeMounts:
            - name: storage
              mountPath: /var/lib/registry
            env:
            - name: REGISTRY_HTTP_ADDR
              value: :5000
            - name: REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY
              value: /var/lib/registry
          volumes:
          - name: storage
            emptyDir: {} # TODO -make this more permanent later

Apply the `Kubernetes` artifacts:

    $ kubectl apply -f local-docker-registry.yaml


## Make the name known to the local `DNS`

Use an editor to add the following line to your `/etc/hosts`:

    127.0.0.1 registry.local


## Restart the cluster

    $ sudo systemctl restart k3s


## A note for `Docker Desktop` users:

This registry isn&rsquo;t configured with `SSL` support. `Docker Desktop` won&rsquo;t connect to
an insecure repository without telling it that this should be allowed. In the
`Docker Desktop` UI, under `settings -> preferences`, you&rsquo;ll need to add the
following block of `yaml`:

    {
      ...
    
      "insecure-registries": [
        "registry.domain.de"
      ]
    }

For more information (not a lot of it, to be honest) follow reference [3] below.


# Use

`Docker` images are tagged by following the convention `[repo/]<image name>:<image
version>`. An image can have an effectively-unlimited number of tags. The `docker
push` command pushes the image to whichever repository is in the tag. This means
that, given the following image (retrieved from the `docker images` command):

    REPOSITORY                                                       TAG                IMAGE ID       CREATED         SIZE
    merlin-phase1-sos                                                0.0.1-SNAPSHOT     78535f397735   2 days ago      246MB

When we execute the command `docker push merlin-phase1-sos`, `Docker` is going to be
unable to determine which repository this came from and you&rsquo;ll get an error. To
push it up to our new local repository, we can simply add a tag:

    $ docker tag merlin-phase1-sos:0.0.1-SNAPSHOT registry.local/merlin-phase1-sos:latest

Then the following command will push the image up to our internal `Kubernetes`
repository:

    $ docker push registry.local/merlin-phase1-sos:latest

We can then deploy it in `Kubernetes` in the usual way, with the exception of
modifying the image name in the `Deployment` artifact. The old container
specification looked like this:

    ...
        spec:
          containers:
            - name: test-app
              image: merlin-phase1-sos:latest
    ...

To tell `Kubernetes` that we want it to look in the new repsitory, we modify it
slightly:

    ...
        spec:
          containers:
            - name: test-app
              image: registry.local/merlin-phase1-sos:latest
    ...


# References

1.  <https://rancher.com/docs/k3s/latest/en/installation/private-registry/>
2.  <https://medium.com/codex/setup-local-integration-environment-with-k3s-and-docker-compose-13fd815765cc>
3.  <https://itnext.io/how-to-setup-a-private-registry-on-k3s-d9283906d16>

