

# RBAC (Role-Based Access Control)

RBAC utilizes roles, which contain permission rules, and role bindings, which
grant the permissions defined in a role to a set of users. This `Role` and
`RoleBinding` are used to grant the `Spring Cloud Kubernetes` library the
permissions needed to read a `ConfigMap`.


## Role


## RoleBiding


# Configuration

A `ConfigMap` is an API object used to store non-confidential data in key-value
pairs. Pods can consume ConfigMaps as environment variables, command-line
arguments, or as configuration files in a volume. A `ConfigMap` allows you to
decouple environment-specific configuration from your container images, so that
your applications are easily portable.

This `ConfigMap` defines the `application.json` file that is passed to the Spring
Cloud Kubernetes `PropertySource` to inject the properties into the service.


# Deployment

A `Deployment` is an API object that manages a replicated application, typically
by running `Pods` with no local state. Each replica is represented by a `Pod`, and
the `Pods` are distributed among the `nodes` of a cluster.

This `Deployment` deploys the `merlin-phase1-sos` service and passes it the
configuration from the previously-created `ConfigMap`.

