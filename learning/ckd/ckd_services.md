

# Services


## Types

-   NodePort: makes an internal service available through a port on the node
    -   TargetPort is the port number on the pod
    -   Port is the port on the service that is mapped to the TargetPort
    -   NodePort is the port on the node through which clients can connect. Must be
        in the range [30000 - 32767].
    -   A `ping` request would travel from an external host, through the `NodePort`,
        through the `Port` on the service and out to `TargetPort` on the pod.
-   ClusterIP: creates a virtual IP address inside the cluster to enable
    communication between services in the cluster. This is a form of service
    discovery between the front-end and back-end.
-   LoadBalancer: balances load across services. This are externally supplied by
    cloud providers.


# YAML Definition Example


## NodePort

    apiVersion: v1
    kind: Service
    metadata:
      name: myapp-service
    
    spec:
      type: NodePort
      ports:
        - targetPort: 80
          port: 80
          nodePort: 30008
      selector:
        app: myapp
        type: front-end


## Cluster IP

    apiVersion: v1
    kind: Service
    metadata:
      name: back-end
    
    spec:
      type: ClusterIP
      ports:
        - targetPort: 80
          port:80
      selector:
        app: my-app
        type: back-end


## Load Balancer

    apiVersion: v1
    kind: Service
    metadata:
      name: myapp-service
    
    spec:
      type: LoadBalancer
      ports:
        - targetPort: 80
          port: 80
          nodePort: 30008


# Commands

To List all services:

    kubectl get services

To create a service:

    kubectl expose pod <pod name> --name <service name> --port <port> --target-port <target port>

    kubectl apply -f <service yaml>

To describe a service:

    kubectl describe service <service name>

