

# Requirements

-   Defined in pod definition file
-   Default is 1 vCPU, 512Mi per container (not per pod)


# YAML Definition Example

`defaults-definition.yaml`

    apiVersion: v1
    kind: LimitRange
    metadata:
      name: mem-limit-range
    spec:
      limits:
      - default:
          memory: 512Mi
        defaultRequest:
          memory: 256Mi
        type: Container

`pod-definition.yaml`

    apiVersion: v1
    kind: Pod
    metadata:
      name: simple-webapp-color
      labels:
        name: simple-webapp-color
    
    spec:
      containers:
      - name: simple-webapp-color
        image: simple-webapp-color
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "1Gi"
            cpu: 1

