

# Docker Storage


## Ephemeral Storage

In the absence of any mounts, `Docker` stores the data in the `Container` layer,
under `/var/lib/docker/containers`.


## Persistent Volumes


### Volume Mounts

These are created in `/var/lib/docker/volumes`


### Bind Mounts

These are stored somewhere on the host OS and the location is specified in the
`docker run` command.

    docke run --mount type=bind,source=/data,target=/var/lib/mysql mysql


## Storage Drivers

Does **not** handle Volumes

-   AUFS
-   ZFS
-   BTRFS
-   Device Mapper
-   Overlay
-   Overlay2


## Volume Drivers

Default is `local`, which stores volumes `/var/lib/docker/volumes`

-   Local
-   Azure File Storage
-   Convoy
-   DigitalOcean Block Storage
-   Flocker
-   gce-docker
-   GlusterFS
-   NetApp
-   RexRay (allows local containers to store data in AWS)
-   PortWorxx
-   VMware vSphere Storage


# Kubernetes Storage

Kubernetes uses a plugin architecture to remain agnostic to underlying
implementations:

Driver types:

-   `CRI` - `Container Runtime Interface`
-   `CNI` - `Container Network Interface`
-   `CSI` - `Contianer Storage Interface`
    (This is not a Kubernetes-specific interface)


# Example YAML Definition:


## Kubernetes Ephemeral Volumes

Volume definitions are tied to the `Pod` definition. This would create a separate
volume on each of the nodes where this `Pod` is deployed:

    apiVersion: v1
    kind: Pod
    metadata:
      name: test-ebs
    spec:
      containers:
      - image: k8s.gcr.io/test-webserver
        name: test-container
        volumeMounts:
        - mountPath: /var/lib/webserver/data
          name: test-volume
      volumes:
      - name: test-volume
        hostPath:
          path: /opt/data
          type: Directory


## Kubernetes Persistent Volumes


### Definition

`persistent-volume-definition.yaml`

    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: pv-log
    spec:
      accessModes:
        - ReadWriteMany
      capacity:
        storage: 100Mi
      hostPath:
        path: /pv/log
      persistentVolumeReclaimPolicy: Retain


### Persistent Volume Claims

`persistent-volume-claim-definition.yaml`

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: myclaim
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 500Mi

Use the `PersistentVolumeClaim` in a `Pod` definition like this:
`pod-definition.yaml`

    apiVersion: v1
    kid: Pod
    metadata:
      name: myPod
    spec:
      containers:
      - name: myFrontEnd
        image: nginx
        volumeMounts:
        - mountPath: "/var/www/html"
          name: mypd
       volumes:
       - name: mypd
         persistentVolumeClaim:
           claimName: myclaim


## Storage Classes

Storage class enables dynamic provisioning of volumes

`storage-class-dinfition.yaml`

    apiVersion: storage.k8s.io/v1
    kind: storageClass
    metadata:
      name: google-storage
    provisioner: kubernetes.io/gce-pd

`pvc-definition.yaml`

    apiVersion:
    kind: PersistentVolumeClaim
    metadata:
      name: myclaim
    spec:
      accessModes:
        - ReadWriteOnce
      storageClassName: google-storage
      resources:
        requests:
          storage: 500Mi

