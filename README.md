```
# Prerequisite : an existing NFS server
showmount -e nfs.lysdemolab.fr
Export list for nfs.lysdemolab.fr:
/export/users *
/export       *



# https://github.com/kubernetes-csi/csi-driver-nfs

# Install CSI driver
helm repo add csi-driver-nfs https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/charts

helm search repo -l csi-driver-nfs
NAME                          CHART VERSION     APP VERSION     DESCRIPTION
csi-driver-nfs/csi-driver-nfs v4.2.0            v4.2.0          CSI NFS Driver for Kubernetes
csi-driver-nfs/csi-driver-nfs v4.1.0            v4.1.0          CSI NFS Driver for Kubernetes
csi-driver-nfs/csi-driver-nfs v4.0.0            v4.0.0          CSI NFS Driver for Kubernetes
csi-driver-nfs/csi-driver-nfs v3.1.0            v3.1.0          CSI NFS Driver for Kubernetes
csi-driver-nfs/csi-driver-nfs v3.0.0            v3.0.0          CSI NFS Driver for Kubernetes
csi-driver-nfs/csi-driver-nfs v2.0.0            v2.0.0          CSI NFS Driver for Kubernetes

helm install csi-driver-nfs csi-driver-nfs/csi-driver-nfs --namespace csi-driver-nfs --create-namespace --version v4.2.0

# csi.storage.k8s.io/provisioner-secret is only needed for providing mountOptions in DeleteVolume
kubectl create secret generic mount-options --from-literal mountOptions="nfsvers=3,vers=3,nolock,rw,proto=tcp,mountproto=tcp"

# Create the sc which is talking to NFS server
k apply -f nfs-csi-sc.yaml

# Create the pv
k apply -f pv-nfs-csi.yaml

# Create the PVC
k apply -f pvc-nfs-csi-static-determined.yaml 
```
