# Kubernetes dashboard installation notes

The k8s dashboard

- The Kubernetes Dashboard is a web-based management interface that enables you to: 
  - deploy and edit containerized applications 
  - assess the status of containerized applications 
  - troubleshoot containerized applications.
  
## Run the following command to create the pods for the k8s Dashboard

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.0/aio/deploy/recommended.yaml`

- This command creates:
  - namespace, `kubernetes-dashboard`
  - ServiceAccount (provides an identity for processes that run in a pod) called kubernetes-dashboard
  - Service (provides a static ip address) called kubernetes-dashboard with ports 443->8443
  - 3 Secrets (configuration file for sensitive data that should not be stored in plain text) 
    - kubernetes-dashboard-certs
    - kubernetes-dashboard-csrf
    - kubernetes-dashboard-key-holder
  - ConfigMap (key/value configuration data) called kubernetes-dashboard-settings
  - Role (defines permission rules within a specific namespace)
  - ClusterRole (defines cluster-wide permission rules)
  - RoleBinding (grants permissions to users within a specific namespace)
  - ClusterRoleBinding (grants permissions to users cluster-wide)
  - Deployment called kubernetes-dashboard 
    - created from image kubernetesui/dashboard:v2.6.0
    - running on port 8443
    - with volumeMounts /certs
  - Service named dashboard-metrics-scraper running on port 8000->8000
  - Deployment called dashboard-metrics-scraper 
    - created from image kubernetesui/metrics-scraper:v1.0.8
    - running on port 8000
    - with volumeMounts /tmp

- The output from the command:


	namespace/kubernetes-dashboard created
	serviceaccount/kubernetes-dashboard created
	service/kubernetes-dashboard created
	secret/kubernetes-dashboard-certs created
	secret/kubernetes-dashboard-csrf created
	secret/kubernetes-dashboard-key-holder created
	configmap/kubernetes-dashboard-settings created
	role.rbac.authorization.k8s.io/kubernetes-dashboard created
	clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard unchanged
	rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
	clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard unchanged
	deployment.apps/kubernetes-dashboard created
	service/dashboard-metrics-scraper created
	deployment.apps/dashboard-metrics-scraper created

## After running apply, running `kubectl get pods -A`

	$ kubectl get pods -A
	NAMESPACE              NAME                                        READY   STATUS    RESTARTS          AGE
	kube-system            coredns-6d4b75cb6d-jtjdp                    1/1     Running   6 (119m ago)      6d3h
	kube-system            coredns-6d4b75cb6d-nh2nt                    1/1     Running   6 (119m ago)      6d3h
	kube-system            etcd-docker-desktop                         1/1     Running   6 (119m ago)      6d3h
	kube-system            kube-apiserver-docker-desktop               1/1     Running   6 (119m ago)      6d3h
	kube-system            kube-controller-manager-docker-desktop      1/1     Running   6 (119m ago)      6d3h
	kube-system            kube-proxy-mmqm5                            1/1     Running   6 (119m ago)      6d3h
	kube-system            kube-scheduler-docker-desktop               1/1     Running   12 (119m ago)     6d3h
	kube-system            storage-provisioner                         1/1     Running   15 (118m ago)     6d3h
	kube-system            vpnkit-controller                           1/1     Running   493 (5m51s ago)   6d3h
	kubernetes-dashboard   dashboard-metrics-scraper-8c47d4b5d-xhlqd   1/1     Running   0                 64s
	kubernetes-dashboard   kubernetes-dashboard-5676d8b865-q7v9n       1/1     Running   0                 64s


## Create a file called `kubernetes-dashboard.yaml` that contains the following:

    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: admin-user
      namespace: kubernetes-dashboard
    ---
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      name: admin-user
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: cluster-admin
    subjects:
    - kind: ServiceAccount
      name: admin-user
      namespace: kubernetes-dashboard


## Run the following command to create the ServiceAccount defined in `kubernetes-dashboard.yaml`

`kubectl apply -f kubernetes-dashboard.yaml`

Output from command:
	serviceaccount/admin-user created
	clusterrolebinding.rbac.authorization.k8s.io/admin-user created

## Create a login token so that you can login to the dashboard
- I think this might need to be recreated everytime you reboot?

	kubectl -n kubernetes-dashboard create token admin-user
	
	Login token:
eyJhbGciOiJSUzI1NiIsImtpZCI6IlRjZTFpTmV5TGdBWXR4NnI3aXpSa3FkSnpWS2dnd09BRk1yQndXQUpKRTAifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjYwNzU3Mjg4LCJpYXQiOjE2NjA3NTM2ODgsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJhZG1pbi11c2VyIiwidWlkIjoiZDdkNTNhZjEtM2IzOC00ZWJlLWFlMTctMDNhYzIxYWM1ZThlIn19LCJuYmYiOjE2NjA3NTM2ODgsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDphZG1pbi11c2VyIn0.lwFK_vzJaGhOwqTI3dGV2nbsrEUac4vEH-QTeuX5D1N-25Vb4jEaWNgqu93VFNJuyAoJt9LhIjY-QPodSLDGKb11I8xAhjwZZ5xl3j6SczVpmUcRWf5mtNFIwCxOXiGhSNLWmXdKI_AH_R9reijOJRc1GZFB6kCklNl1Zn4Rwfi-lZZ-H5TVDOUZAo4-uTL6WA8H9Qd-7f9EM7NrRBokyfmYPXCE-YJltimmflVpEXGI-M8a-0D45gEUBXSvk_pMyCr3QHUn4wsa6r0awLbBXqeDAYi-E1JkU2DsvlxyMlobM2Rg5zxWZew5ju6x4Yrl5Ev2xL2H8lKrTeYZp_aLog

Copy and paste the token text into notepad and save it.

## Start the proxy
- This command should be run in the background.  This command needs to be re-run when you reboot.

	kubectl proxy
	Starting to serve on 127.0.0.1:8001
	
# Accessing the Dashboard

To access the dashboard, navigate to:
`http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/`.
You should be presented with the login screen.

Copy and past the token and click the `Sign in` button. This should present you
with the main dashboard screen.

Do you have to recreate the token every time you reboot?
