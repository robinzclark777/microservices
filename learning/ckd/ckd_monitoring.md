

# Monitoring


## Metrics Server

-   Metrics Server is an in-memory monitoring server
-   Metrics Server doesn&rsquo;t persist data to disk. For that, we need to use
    Prometheus or some other metrics database.
-   Not deployed by default


## Logging

-   Simple implementation provided, relies on add-ons for advanced features


# Commands


## Metrics

To see CPU and Memory consumption of each node:

    kubectl top node

To see CPU and Memory consumption of each pod:

    kubectl top pod


## Logs

To tail logs for a specific pod:

    kubectl logs -f <pod name>

To tail logs for a specific container within a multi-container pod:

    kubectl logs -f <pod name> <container name>

