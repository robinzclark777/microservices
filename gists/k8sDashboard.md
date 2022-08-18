Kubernetes dashboard installation notes

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.0/aio/deploy/recommended.yaml

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


kubectl apply -f kubernetes-dashboard.yaml

serviceaccount/admin-user created
clusterrolebinding.rbac.authorization.k8s.io/admin-user created

kubectl -n kubernetes-dashboard create token admin-user

Login token:
eyJhbGciOiJSUzI1NiIsImtpZCI6IlRjZTFpTmV5TGdBWXR4NnI3aXpSa3FkSnpWS2dnd09BRk1yQndXQUpKRTAifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjYwNzU3Mjg4LCJpYXQiOjE2NjA3NTM2ODgsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJhZG1pbi11c2VyIiwidWlkIjoiZDdkNTNhZjEtM2IzOC00ZWJlLWFlMTctMDNhYzIxYWM1ZThlIn19LCJuYmYiOjE2NjA3NTM2ODgsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDphZG1pbi11c2VyIn0.lwFK_vzJaGhOwqTI3dGV2nbsrEUac4vEH-QTeuX5D1N-25Vb4jEaWNgqu93VFNJuyAoJt9LhIjY-QPodSLDGKb11I8xAhjwZZ5xl3j6SczVpmUcRWf5mtNFIwCxOXiGhSNLWmXdKI_AH_R9reijOJRc1GZFB6kCklNl1Zn4Rwfi-lZZ-H5TVDOUZAo4-uTL6WA8H9Qd-7f9EM7NrRBokyfmYPXCE-YJltimmflVpEXGI-M8a-0D45gEUBXSvk_pMyCr3QHUn4wsa6r0awLbBXqeDAYi-E1JkU2DsvlxyMlobM2Rg5zxWZew5ju6x4Yrl5Ev2xL2H8lKrTeYZp_aLog

kubectl proxy
Starting to serve on 127.0.0.1:8001
