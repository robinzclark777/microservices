

# Controllers


## Replication Controller

-   `apiVersion` is `v1`
-   `kind` = `ReplicaController`
-   Replica Set is replacing Replica Controller
-   Allows us to run multiple instances of a container simultaneously
-   Automated Scaling and Load Balancing


## Replica Set

-   `apiVersion` is `apps/v1`
-   `kind` = `ReplicaSet`
-   Similar to Replication Controller
-   Selector is required


# Example Yaml Definition

    apiVersion: apps/v1
    kind: ReplicaSet


# Commands

To List `ReplicaSets`:

    kubectl get replicaset [-o wide]

To Create a `ReplicaSet`:

    kubectl create -f <replicaset yaml>

To update a `ReplicaSet`:

    kubectl replace -f <replicaset yaml>

To delete a `ReplicaSet`:

    kubectl delete replicaset <replicaset name>

To scale a `ReplicaSet`:

    kubectl scale --replicas=6 -f <replicaset yaml>
    # OR
    # <type> = replicaset
    kubectl scale --replicas=6 <type> <replicaset name>

