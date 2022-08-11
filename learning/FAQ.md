

# FAQ

A list of issues encountered and their resolution

-   [k3s in WSL] `helm install -n merlin-phase1 kafka bitnami/kafka` gives `INSTALLATION FAILED:
      failed to download bitnami/kafka`
    1.  Verify that you can access the cluster **without** `sudo`
        
            $ kubectl get pods -A
        
        If this command is successful, then skip to step #3.
    
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

