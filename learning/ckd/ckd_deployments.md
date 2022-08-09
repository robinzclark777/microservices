

# Deployments

-   With Deployments you can easily edit any field/property of the POD
    template. Since the pod template is a child of the deployment specification,
    with every change the deployment will automatically delete and create a new
    pod with the new changes. So if you are asked to edit a property of a POD part
    of a deployment you may do that simply by running the command


## Rollouts and Versioning

-   `Rolling Update` is the default deployment strategy
-   When a new deployment is created, k8s creates a new `ReplicaSet` and deploys the
    pods. As the new `ReplicaSet` is scaled up, the original is scaled down.


# YAML Definition Example

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: myapp-deployment
      labels:
        app: myapp
        type: front-end
    spec:
      template:
        metadata:
          name: myapp-prod
          labels:
            app: myapp
            type: front-end
        spec:
          containers:
            - name: nginx-container
              image: nginx
      replicas: 3
      selector:
        matchLabels:
          type: front-end


# Commands

To List `Deployments`:

    kubectl get deployments [-o wide]

To Create a `Deployment`:

    kubectl create -f <deployment yaml>

To Create a `Deployment yaml` file:

    kubectl create deployment --image=<image name> <image name> \
            --replicas=<replica count> --dry-run=client -o yaml > <image name>-deployment.yaml

To update a `Deployment`:

    kubectl replace -f <deployment yaml>

To edit a running `Deployment`:

    kubectl edit deployment my-deployment

To delete a `Deployment`:

    kubectl delete deployment <deployment name>

To see a `Rollout` status:

    kubectl rollout status deployment <deployment name>

To update a `Deployment`:

    kubectl set image deployment.v1.apps/<deployment name> <image name>=<image name>:<version>

To undo a `Rollout`:

    kubectl rollout undo <deployment name>

To see changes after `Rollout` or undo:

    kubectl get replicasets

