

# FAQ

A list of issues encountered and their resolution


## [k3s in WSL] `kubectl` only works as the root user or requires `sudo`:

1.  To verify that you can access the cluster **without** `sudo`
    
        $ kubectl get pods -A
    
    The specifics of which pods are running aren&rsquo;t relevant, as long as you see
    **some** `Pods` running. If this fails with an error, then continue on to Step 2.

2.  Verify that `$HOME.kube/config` exists, has the correct owner group and permissions:
    
        ls -l ~/.kube/config
    
    The file should be owned by the non-root user, the group should have the
    same value and the permissions should be 600 (-rw--&#x2013;&#x2014;).
    
    -   if the file doesn&rsquo;t exist:
        
            $ sudo cat /etc/rancher/k3s/k3s.yaml > ~/.kube/config
    
    -   If it&rsquo;s still owned by the `root` user:
        
            $ sudo chown $USER:$USER ~/.kube/config
    
    -   If the permissions are incorrect:
        
            $ chmod 600 ~/.kube/config
    
    If the `helm install` command still fails, the continue on to step 3.

3.  `kubectl` may be trying to read the wrong `kubeconfig` file. Try setting the
    `KUBECONFIG` environment variable to point to your local `$HOME/.kube/config`.
    
        $ export KUBECONFIG=$HOME/.kube/config
    
    Then try the `helm install` command again. If this fixes the problem, then
    copy the `export` command to the bottom of your `$HOME/.bashrc` file.


## [k3s on WSL] `helm repo list` gives and error like the one below:

    oras.land/oras-go/pkg/auth/docker.NewClientWithDockerFallback
        oras.land/oras-go@v1.2.0/pkg/auth/docker/client.go:80
    helm.sh/helm/v3/pkg/registry.NewClient
        helm.sh/helm/v3/pkg/registry/client.go:83
    main.newRootCmd
        helm.sh/helm/v3/cmd/helm/root.go:152
    main.main
        helm.sh/helm/v3/cmd/helm/helm.go:66
    runtime.main
        runtime/proc.go:255
    runtime.goexit
        runtime/asm_amd64.s:1581

`helm` is not able to read the repository file. Verify that
`$HOME/.config/helm/repositories.yaml` exists:

    ls ~/.config/helm/repositories.yaml

If it&rsquo;s not there, then create it and paste in the following contents:

    apiVersion: ""
    generated: "0001-01-01T00:00:00Z"
    repositories:
    - caFile: ""
      certFile: ""
      insecure_skip_tls_verify: false
      keyFile: ""
      name: kubernetes-dashboard
      pass_credentials_all: false
      password: ""
      url: https://kubernetes.github.io/dashboard/
      username: ""
    - caFile: ""
      certFile: ""
      insecure_skip_tls_verify: false
      keyFile: ""
      name: bitnami
      pass_credentials_all: false
      password: ""
      url: https://charts.bitnami.com/bitnami
      username: ""
    - caFile: ""
      certFile: ""
      insecure_skip_tls_verify: false
      keyFile: ""
      name: kafka-ui
      pass_credentials_all: false
      password: ""
      url: https://provectus.github.io/kafka-ui
      username: ""

