

# Daemon Sets

-   Daemon sets ensure that one copy of a pod is always present on every node
-   This is valuable for things like health checks or Kube-proxy
-   Very similar to `ReplicaSet`
-   Uses node affinity and default scheduler


# YAML Definition Example

    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: monitoring-daemon
    spec:
      selector:
        matchLabels:
          app: monitoring-agent
      template:
        metadata:
          labels:
            app: monitoring-agent
        spec:
          containers:
          - name: monitoring-agent
            image: monitoring-agent


# Commands

To create a `DaemonSet`:

    kubectl apply -f <daemonset definition file>

To list `DaemonSets`:

    kubectl get daemonsets

To describe a `DaemonSet`:

    kubectl describe daemonsets <daemonset name>

