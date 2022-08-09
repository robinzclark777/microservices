

# Private Repositories

To log into a private registry:

    docker login private-registry.io

To create `k8s` secret for a `Registry`:

    kubectl create secret docker-registry <registry-name> \
    	--docker-server=<registry URL> \
    	--docker-username=<registry user id> \
    	--docker-password=<registry password> \
    	--docker-email=<email address>


# Example YAML Definition

    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx-pod
    spec:
      containers:
      - name: nginx
        image: private-registry.io/apps/internal-app
      imagePullSecrets:
      - name: private-registry-credentials

