

# Prerequisites

-   `helm` is installed on the local machine
-   User has permissions to install pods on the cluster


# Create a Target Kubernetes Namespace

This step is only necessary if the namespace does not already exist.

    kubectl create namespace merlin-phase1


# Add Rook Helm Repository

This step is only necessary if the Bitnami repo wasn&rsquo;t previously added

    helm repo add rook-release https://charts.rook.io/release


# Install the Image Using Helm

    elm install -n merlin-phase1 rook-ceph rook-release/rook-ceph


# To Uninstall Ceph

    helm uninstall -n merlin-phase1 rook-ceph-operator...

