

# Configuration


## Environment Variables

-   Can be set directly on the `Pod` using `env[n].key:` and `env[n].value:`
-   Can also be supplied by `ConfigMap` or `Secret` using `env[n].valueFrom`


# YAML Definition Example

Setting environment variables:

    apiVersion: v1
    kind: Pod
    metadata:
      name: simple-webapp-color
    spec:
      containers:
        - name: simple-webapp-color
          image: simple-webapp-color
          ports:
            - containerPort 8080
          env:
            - name: APP_COLOR
            - value: green

Defining a `ConfigMap`:

    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: app-config
    data:
      APP_COLOR: blue
      APP_MODE: prod

Using a `ConfigMap` in a `Pod` definition:

    apiVersion: v1
    kind: Pod
    metadata:
      name: dapi-test-pod
    spec:
      containers:
        - name: test-container
          image: k8s.gcr.io/busybox
          command: [ "/bin/sh", "-c", "env" ]
          envFrom:
          - configMapRef:
              name: special-config
      restartPolicy: Never


# Commands

To create a `ConfigMap` from literal values:

    kubectl create configmap <configmap name> --from-literal <key>=<value> \
            [--from-literal <key>=<value>]

To create a `ConfigMap` from a file:
\#+begin<sub>src</sub> shell
  kubectl create configmap <configmap name> &#x2013;from-file <file path>
\#+end<sub>sr</sub>

