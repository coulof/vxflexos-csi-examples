# Enable CSI in k8s cluster

The [CSI specification 1.0 is GA](https://kubernetes.io/blog/2019/01/15/container-storage-interface-ga/) since kubernetes release 1.13 but disabled by default.

In the following video, we will show how to enable it on our three nodes cluster with one master (k8s-master) and two minions(k8s-minion-1 & k8s-minion-2).

To enable the driver there are 3 main steps :
1. On every nodes, configure kubelet service to use CSI with: 
   * `/var/lib/kubelet/config.yaml`
    ```
    VolumeSnapshotDataSource: true
    KubeletPluginsWatcher: true
    CSINodeInfo: true
    CSIDriverRegistry: true
    BlockVolume: true
    CSIBlockVolume: true
    ```
   *  and `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf` :
    ```
    --allow-privileged=true --feature-gates=VolumeSnapshotDataSource=true,KubeletPluginsWatcher=true,CSINodeInfo=true,CSIDriverRegistry=true,BlockVolume=true,CSIBlockVolume=true
    ```

2. On every nodes, configure docker service \(`/etc/systemd/system/multi-user.target.wants/docker.service`\)to use `MountFlags=shared`
3. On master node, configure `/etc/kubernetes/manifests/kube-apiserver.yaml`, `/etc/kubernetes/manifests/kube-controller-manager.yaml`, and `/etc/kubernetes/manifests/kube-scheduler.yaml` to use the CSI flags
   ```
   - --feature-gates=VolumeSnapshotDataSource=true,KubeletPluginsWatcher=true,CSINodeInfo=true,CSIDriverRegistry=true,BlockVolume=true,CSIBlockVolume=true
   ```

For detailed configuration, you can refer to the [official CSI documentation](https://kubernetes.io/docs/concepts/storage/volumes/#csi)

[![asciicast](https://asciinema.org/a/238625.svg)](https://asciinema.org/a/238625?speed=2)

# Install VxFlexOS CSI Driver
Soon

# Use postgres HA + CSI Driver
Soon

# Infinite scaling with Couchbase and VxFlex OS
Soon