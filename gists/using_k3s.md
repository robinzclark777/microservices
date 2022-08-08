The information here was taken from
<https://rancher.com/docs/k3s/latest/en/quick-start/>


# Prerequisites

-   The target machine must have a working `Docker` install


# Installing `k3s` on Master Node


## Retrieve and Install k3s

    curl -sfL https://get.k3s.io | sh -


## Copy the Root kubeconfig.yaml to Local User

    sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config


## Verify Installation

    kubectl get pods -A


# Installing `k3s` on Worker nodes


## Copy `k3s` Token From Master Node

    sudo cat /var/lib/rancher/k3s/server/node-token


## Retrieve and Install k3s

On the worker node:

    curl -sfL https://get.k3s.io | K3S_URL=https://master-node:6443 \
        K3S_TOKEN=K100a24f0048bd0f4fca2dcba3a06e0e4014ca2067cd06ed1ec1c89f4ecbbea9e77::server:23e30fda2c63734f687242fe5822a732 sh -


## Verify Additional Node

On the master node:

    kubectl get nodes


# Installing Locally-Built Images on k3s


## Copy the image from the local Docker daemon to a `.tar` file

    docker save --output <docker-image-name>.tar <docker-image-name>:<version>


## Install Image in `k3s`

    sudo k3s ctr images import <docker-image-name>.tar


## Delete Image from `k3s`

    sudo k3s ctr images remove docker.io/library/<docker-image-name>:<version>

