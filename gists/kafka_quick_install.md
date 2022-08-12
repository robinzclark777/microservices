

# Prerequisites

-   `helm` is installed on the local machine
-   User has permissions to install pods on the cluster


# Create a Target Kubernetes Namespace

This step is only necessary if the namespace does not already exist.

    kubectl create namespace merlin-phase1


# Add Bitnami Helm Repository

This step is only necessary if the Bitnami repo wasn&rsquo;t previously added

    helm repo add bitnami https://charts.bitnami.com/bitnami


# Install the Image Using Helm

This command deploys `Kafka`. The `deleteTopicEnable` setting allows us to remove
topics. For a complete list of available parameters, see Reference [1].

    helm install -n merlin-phase1 --set deleteTopicEnable=true kafka bitnami/kafka


# To Uninstall Kafka

    helm uninstall -n merlin-phase1 kafka


# References

[1] - <https://github.com/bitnami/charts/tree/master/bitnami/kafka/#installing-the-chart>

