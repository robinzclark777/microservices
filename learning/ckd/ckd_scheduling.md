

# Scheduling

-   Pods are automatically scheduled at creation time if their definition doesn&rsquo;t
    include a `nodeName` attribute
-   Including the `nodeName` attribute causes the pod to be assigned to that
    particular node
-   Pods can be assigned to a specific node by creating a `Binding`


## Multiple Scheduling+ We can write our own k8s scheduler and start it as a service

-   K8s can use multiple schedulers
-   by default, it uses `default-scheduler`
-   The `leader-elect=true` argument is specified to tell k8s that when multiple
    pods of the scheduler are running, it should select one to use and put the
    others in standby


# YAML Definition Example

`pod-bind-definition.yaml`

    apiVersion: v1
    kind: Binding
    metadata:
      name: nginx
    
    target:
      apiVersion: v1
      kind: Node
      name: node02


# Commands

    curl --header "Content-Type:application/json" --request POST \
         --data '{"apiVersion":"v1", "kind":"Binding" ...}'

To see scheduling events:

    kubctl get events

To see logs:

    kubctl logs <scheduler name> -n kube-system


# References

-   <https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/>

