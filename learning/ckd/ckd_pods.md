-   `Pods` are the most fine-grained object that can be created in Kubernetes
-   Scale the application by adding pods
-   Groups containers and helper-containers together to allow them to share
    resources like filesystems. Multi-use pods are a rare use-case.


# Multi-container `Pods`

-   Multi-container pods typically follow one of three design patterns:
    1.  Sidecar
    2.  Adapter
    3.  Ambassador

-   Containers that run and then exit while the `Pod` continues to run are called `InitContainers`.


# YAML Definition Example

    apiVersion: v1
    kind: Pod
    metadata:
      name: myapp-pod
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: busybox:1.28
        command: ['sh', '-c', 'echo The app is running! && sleep 3600']
      initContainers:
      - name: init-myservice
        image: busybox:1.28
        command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
      - name: init-mydb
        image: busybox:1.28
        command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']apiVersion: v1


# Commands

Listing `Pods`

    kubectl get pods [-n <namespace name>]

To create a `Pod`:

    kubectl create -f <pod definition file>
    # OR
    kubectl apply -f <pod definition file>

To retrieve the definition of a live `Pod`:

    kubectl get pod <pod name> -o yaml > <pod definition file>

To update a running `Pod`:

    kubectl edit pod <pod name>

To create a `.yaml` file for a given container image:

    kubectl run <pod name> --image=<image identifier> --dry-run=client -o yaml > <pod definition file>

