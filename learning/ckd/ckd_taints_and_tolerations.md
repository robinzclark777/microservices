

# Taints and Tolerations

-   Taints are set on nodes
-   Tolerations are set on pods
-   pods will only be deployed to nodes that can tolerate their taints
-   Possible taint effects:
    1.  `NoSchedule`: Do not schedule pods on this node if they can&rsquo;t tolerate the
        taint
    2.  `PreferNoSchedule`: Schedule pods that can&rsquo;t tolerate this taint on other
        nodes if possible
    3.  `NoExecute`: `NoSchedule` plus evict any previously-scheduled pods that can&rsquo;t
        tolerate the taint


# YAML Definition Example

The `tolerations` block of `yaml` is used to define tolerations on a pod.

`pod-definition.yaml`

    apiVersion: v1
    kind: Pod
    metadata:
      name: myapp-pod
    spec:
      containers:
        - name: nginx-container
          image: nginx
    
      tolerations:
        - key: "app"
          operator: "Equal"
          value: "blue"
          effect: NoSchedule


# Commands

To taint a node:

    kubectl taint nodes <node name> <key>=<value>:<taint effect>

To remove a taint from a node:

    kubectl taint nodes <node name> <key>=<value>:<taint effect>-

To make a pod tolerant:

    kubectl apply -f <pod definition file>

To see a taint:

    kubectl describe node <node name> | grep Taint

