

# Node Affinity

-   ensures that pods are hosted on particular nodes
-   needed because node selectors do not support logical `or`
-   Node Affinity Types:
    -   requiredDuringSchedulingIgnoredDuringExecution
    -   preferredDuringSchedulingIgnoredDuringExecution:
    -   requiredDuringSchedulingRequiredDuringExecution:


# YAML Definition Example

`pod-definition.yaml`

    apiVersion: v1
    kind: Pod
    
    metadata:
      name: myapp-pod
    
    spec:
      containers:
      - name: data-processor
        image: data-processor
    
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: size
                operator: NotIn
                values:
                - Small
                - Medium


# Commands

Typical pod CRUD commands are used:
[CKD Pods](./ckd_pods.md)

Also, the labels/selectors commands may be useful
[CKD Labels and Selectors](./ckd_labels_and_selectors.md)

