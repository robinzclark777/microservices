

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

    helm install -n merlin-phase1 kafka bitnami/kafka


# To Uninstall Kafka

    helm uninstall -n merlin-phase1 kafka

