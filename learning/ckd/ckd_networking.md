

# Basics


## Switching


## Routing

To add a route:

    ip route add 192.168.2.0/24 via 192.168.1.1


## Default Gateway

To define a `Default Gateway`

    ip route add default via 192.168.2.1

In Linux, network packet forwarding is disabled by default. This means that if
the gateway is a Linux installation, we need to enable ip-forwarding. This is
done in the file `/proc/sys/net/ipv4/ip_forward` by setting its contents to the
value of 1.

    echo 1 > /proc/sys/net/ipv4/ip_forward

To find the port that a particular process is running on:

    netstat -natulp | grep <process name>


## DNS Configurations


## CoreDNS Introduction


## Network Namespaces

Used by Docker to implement network isolation

To create a network namespace:

    ip netns add <namespace name>

To list network namespaces:

    ip netns

To list interfaces on the host:

    ip link

To run commands within a namespace:

    ip netns exec <namespace> <command>
    # for example: ip netns exec red ip link
    #OR#
    ip -n <namespace> <ip subcommand>
    # for example: ip -n red link

Example that creates two network namespaces, allowing them to communicate:

    # create a virtual network interface
    ip link add veth-red type veth peer name veth-blue
    # assign it to a namespace
    ip link set veth-red netns red
    # do the same for another
    ip link set veth-blue netns blue
    # assign them static IPs
    ip -n red addr add 192.168.15.1 dev veth-red
    ip -n blue addr add 192.168.15.2 dev veth-blue
    # bring the links up
    ip -n red link set veth-red up
    ip -n red link set veth-blue up

To create a virtual switch:

    ip link add <virtual switch name> type bridge
    
    # attach virtual NICs to switch
    ip link add <virtual NIC> type with peer name <bridge name>
    ip link set <virtual NIC> netns <namespace>
    ip link set <bridge name> master <virtual switch name>
    
    # connect the virtual networks to the host
    ip link add <ip-address>/24 dev <virtual switch name>


# Docker Networking


## Network Types

-   `None` - inaccessible
-   `Host` - shares the host networking
-   `Bridge` - Created by `Docker`, named `docker0`


# Kubernetes Networking

`Kubernetes` networking addresses four concerns:

-   `Containers` within a `Pod` use networking to communicate via loopback.
-   Cluster networking provides communication between different `Pods`.
-   The `Service` resource lets you expose an application running in `Pods` to be
    reachable from outside your cluster.
-   You can also use `Services` to publish services only for consumption inside your
    cluster.


## Note about Ingress

An Ingress does not expose arbitrary ports or protocols. Exposing services other
than HTTP and HTTPS to the internet typically uses a service of type
Service.Type=NodePort or Service.Type=LoadBalancer. 


## Required Ports

-   `kube-apiserver` uses 6443
-   `kubelet` uses 10250
-   `kube-scheduler` uses 10251
-   `kube-controller-manager` uses 10252
-   `etcd` uses 2379
-   worker nodes make ports 30000-32767 available for applications


## Pod Networking


### Expectations

-   Every `Pod` should get its own IP address
-   Every `Pod` should be able to communicate with every other `Pod` on the same host
-   Every `Pod` should be able to communicate with every other `Pod` on other `Nodes`
    without using `NAT`


### CNI

-   This is a standard which delegates the process of setting up the container

networks.

-   Configured on `kubelet` and `/etc/cni/net.d/10-bridge.conf`

    {
      "cniVersion": "0.2.0",
      "name": "mynet",
      "type": "bridge",
      "bridge": "cni0",
      "isGateway": true,
      "ipMasq": true,
      "ipam" :  {
         "type": "host-local",
         "subnet": "10.22.0.0/16",
         "routes": [
           { "dst": "0.0.0.0/0" }
         ]
    }

