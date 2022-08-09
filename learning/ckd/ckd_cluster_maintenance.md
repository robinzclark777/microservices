

# Cluster Maintenance


## OS Upgrades

-   `Nodes` can be gracefully removed from the cluster by `draining` the `Pods` off
    to another node
-   `Nodes` are automatically `cordoned` off as part of the `drain` process
-   When the upgrade is completed, the `Node` can be placed back into service by
    `uncordoning` it with the command below


## Kubernetes Releases

-   `K8s` follows semantic versioning schema for its version numbering
-   `kube-apiserver`, `controller-manager`, `kube-scheduler`, `kubelet`, `kube-proxy` and
    `kubectl` are released together and share a version number
-   `ETCD Cluster` and `CoreDNS` each get their own version numbers


## Cluster Upgrades

-   K8s supports 3 minor versions at any given time
-   The best practice is to upgrade a single minor version at a time


# Commands

To drain a `Node`:

    kubectl drain <node name>

To cordon a `Node`:

    kubectl cordon <node name>

To uncordon a `Node`:

    kubectl uncordon <node name>

To get an upgrade plan:

    kubeadm upgrade plan

To complete the upgrade of the `kubeadm` upgradable components:

    kubeadm upgrade apply <new version number>

