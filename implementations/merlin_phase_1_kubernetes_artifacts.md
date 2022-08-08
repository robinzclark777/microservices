

# RBAC (Role-Based Access Control)

RBAC utilizes roles, which contain permission rules, and role bindings, which
grant the permissions defined in a role to a set of users. This `Role` and
`RoleBinding` are used to grant the `Spring Cloud Kubernetes` library the
permissions needed to read a `ConfigMap`.


## Role

    ---
    kind: Role
    apiVersion: rbac.authorization.k8s.io/v1
    
    metadata:
      name: namespace-reader
    rules:
      - apiGroups: [""]
        resources: ["configmaps", "pods", "services", "endpoints", "secrets"]
        verbs: ["get", "list", "watch"]


## RoleBiding

    ---
    kind: RoleBinding
    apiVersion: rbac.authorization.k8s.io/v1
    
    metadata:
      name: namespace-reader-binding
    subjects:
      - kind: ServiceAccount
        name: default
        apiGroup: ""
    roleRef:
      kind: Role
      name: namespace-reader
      apiGroup: ""


# Configuration

A `ConfigMap` is an API object used to store non-confidential data in key-value
pairs. Pods can consume ConfigMaps as environment variables, command-line
arguments, or as configuration files in a volume. A `ConfigMap` allows you to
decouple environment-specific configuration from your container images, so that
your applications are easily portable.

This `ConfigMap` defines the `application.json` file that is passed to the Spring
Cloud Kubernetes `PropertySource` to inject the properties into the service.

    ---
    kind: ConfigMap
    apiVersion: v1
    
    metadata:
      name: merlin-phase1-sos-config
    data:
      application.json:
        ' {
        "mil.afdcgs.merlin.sos.kafka.bootstrap-server": "kafka-0.kafka-headless.merlin-phase1.svc.cluster.local:9092",
        "mil.afdcgs.merlin.sos.kafka.partition-count": "1",
        "mil.afdcgs.merlin.sos.kafka.replica-count": "1"
      }'


# Deployment

A `Deployment` is an API object that manages a replicated application, typically
by running `Pods` with no local state. Each replica is represented by a `Pod`, and
the `Pods` are distributed among the `nodes` of a cluster.

This `Deployment` deploys the `merlin-phase1-sos` service and passes it the
configuration from the previously-created `ConfigMap`.

    ---
    kind: Deployment
    apiVersion: apps/v1
    
    metadata:
      name: merlin-phase1-sos
      labels:
        name: merlin-phase1-sos
    spec:
      replicas: 1
      selector:
        matchLabels:
          name: merlin-phase1-sos
      template:
        metadata:
          labels:
            name: merlin-phase1-sos
        spec:
          containers:
            - name: test-app
              image: merlin-phase1-sos:latest
              imagePullPolicy: Never
              env:
                - name: SPRING_APPLICATION_JSON
                  valueFrom:
                    configMapKeyRef:
                      name: merlin-phase1-sos-config
                      key: application.json

