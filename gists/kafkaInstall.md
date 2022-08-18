Kafka Installation Notes:

$ helm repo list
NAME            URL
bitnami         https://charts.bitnami.com/bitnami
kafka-ui        https://provectus.github.io/kafka-ui

rclark-admin@RCLARK-LT MINGW64 /d/merlin
$ helm install -n merlin-phase1 --set deleteTopicEnable=true kafka bitnami/kafka
NAME: kafka
LAST DEPLOYED: Wed Aug 17 12:44:43 2022
NAMESPACE: merlin-phase1
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: kafka
CHART VERSION: 18.0.8
APP VERSION: 3.2.1

** Please be patient while the chart is being deployed **

Kafka can be accessed by consumers via port 9092 on the following DNS name from within your cluster:

    kafka.merlin-phase1.svc.cluster.local

Each Kafka broker can be accessed by producers via port 9092 on the following DNS name(s) from within your cluster:

    kafka-0.kafka-headless.merlin-phase1.svc.cluster.local:9092

To create a pod that you can use as a Kafka client run the following commands:

    kubectl run kafka-client --restart='Never' --image docker.io/bitnami/kafka:3.2.1-debian-11-r4 --namespace merlin-phase1 --command -- sleep infinity
    kubectl exec --tty -i kafka-client --namespace merlin-phase1 -- bash

    PRODUCER:
        kafka-console-producer.sh \

            --broker-list kafka-0.kafka-headless.merlin-phase1.svc.cluster.local:9092 \
            --topic test

    CONSUMER:
        kafka-console-consumer.sh \

            --bootstrap-server kafka.merlin-phase1.svc.cluster.local:9092 \
            --topic test \
            --from-beginning

$ kubectl get pods -A
NAMESPACE              NAME                                        READY   STATUS    RESTARTS        AGE
kube-system            coredns-6d4b75cb6d-jtjdp                    1/1     Running   6 (147m ago)    6d3h
kube-system            coredns-6d4b75cb6d-nh2nt                    1/1     Running   6 (147m ago)    6d3h
kube-system            etcd-docker-desktop                         1/1     Running   6 (147m ago)    6d3h
kube-system            kube-apiserver-docker-desktop               1/1     Running   6 (147m ago)    6d3h
kube-system            kube-controller-manager-docker-desktop      1/1     Running   6 (147m ago)    6d3h
kube-system            kube-proxy-mmqm5                            1/1     Running   6 (147m ago)    6d3h
kube-system            kube-scheduler-docker-desktop               1/1     Running   12 (147m ago)   6d3h
kube-system            storage-provisioner                         1/1     Running   15 (147m ago)   6d3h
kube-system            vpnkit-controller                           1/1     Running   494 (16m ago)   6d3h
kubernetes-dashboard   dashboard-metrics-scraper-8c47d4b5d-xhlqd   1/1     Running   0               29m
kubernetes-dashboard   kubernetes-dashboard-5676d8b865-q7v9n       1/1     Running   0               29m
merlin-phase1          kafka-0                                     1/1     Running   1 (8m22s ago)   8m35s
merlin-phase1          kafka-zookeeper-0                           1/1     Running   0               8m35s

Kafka UI Installation Notes

$ helm install kafka-ui kafka-ui/kafka-ui --set envs.config.KAFKA_CLUSTERS_0_NAME=local \
>     --set envs.config.KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS=kafka-0.kafka-headless.merlin-phase1.svc.cluster.local:9092
NAME: kafka-ui
LAST DEPLOYED: Wed Aug 17 12:55:03 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=kafka-ui,app.kubernetes.io/instance=kafka-ui" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:8080
  
  rclark-admin@RCLARK-LT MINGW64 /d/merlin
$ kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
kafka-ui-5d46b99bf-7l86k   1/1     Running   0          82m

rclark-admin@RCLARK-LT MINGW64 /d/merlin
$ kubectl --namespace default port-forward kafka-ui-5d46b99bf-7l86k 8081:8080
Forwarding from 127.0.0.1:8081 -> 8080
Forwarding from [::1]:8081 -> 8080

