

# Namespaces

-   `default`, `kube-system`, `kube-public` - created automatically
-   Resource limits/quotas can be defined for each namespace
-   Services are addressable following this format: `<service
      name>.<namespace>.svc.cluster.local`. For example,
    `mysql.dev.svc.cluster.local`.
-   Namespaces can be defined with a YAML definition:

    apiVersion: v1
    kind: Namespace
    metadata:
      name: dev

-   Namespaces can be assigned on the command line or specified within a resource
    definition yaml file.

    apiVersion: v1
    kind: Pod
    
    metadata:
      name: myapp-pod
      namespace: dev
      labels:
        app: myapp
        type: front-end
    
    spec:
      containers:
        - name: nginx-container
          image: nginx


# Commands

Creating a resource in a specific namespace:

    kubectl create -f <yaml> --namespace=<namespace>

Creating a namespace:

    kubectl create namespace <namespace name> [--dry-run]

Setting the session default namespace:

    kubectl config set-context $(kubectl config current-context) --namespace=dev

Lising resources from non-default namespaces:

    kubectl get pods --namespace=dev
    kubectl get pods --all-namespaces

