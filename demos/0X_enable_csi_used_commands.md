lolcat -a 00_enable_csi_intro.md 

ssh k8s-master kubectl get nodes


lolcat -a 01_enable_csi_all_nodes.md

vi /var/lib/kubelet/config.yaml

VolumeSnapshotDataSource: true
KubeletPluginsWatcher: true
CSINodeInfo: true
CSIDriverRegistry: true
BlockVolume: true
CSIBlockVolume: true


scp /var/lib/kubelet/config.yaml k8s-minion-1:/var/lib/kubelet/config.yaml
scp /var/lib/kubelet/config.yaml k8s-minion-2:/var/lib/kubelet/config.yaml

vi /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

--allow-privileged=true --feature-gates=VolumeSnapshotDataSource=true,KubeletPluginsWatcher=true,CSINodeInfo=true,CSIDriverRegistry=true,BlockVolume=true,CSIBlockVolume=true

scp /etc/systemd/system/kubelet.service.d/10-kubeadm.conf k8s-minion-1:/etc/systemd/system/kubelet.service.d/10-kubeadm.conf
scp /etc/systemd/system/kubelet.service.d/10-kubeadm.conf k8s-minion-2:/etc/systemd/system/kubelet.service.d/10-kubeadm.conf

vi /etc/systemd/system/multi-user.target.wants/docker.service

MountFlags=shared

scp /etc/systemd/system/multi-user.target.wants/docker.service k8s-minion-1:/etc/systemd/system/multi-user.target.wants/docker.service
scp /etc/systemd/system/multi-user.target.wants/docker.service k8s-minion-2:/etc/systemd/system/multi-user.target.wants/docker.service


lolcat -a 02_enable_csi_master_nodes.md

vi /etc/kubernetes/manifests/kube-controller-manager.yaml
- --feature-gates=VolumeSnapshotDataSource=true,KubeletPluginsWatcher=true,CSINodeInfo=true,CSIDriverRegistry=true,BlockVolume=true,CSIBlockVolume=true

vi /etc/kubernetes/manifests/kube-apiserver.yaml

vi /etc/kubernetes/manifests/kube-scheduler.yaml

pssh -i -h ~/k8s.hosts -- 'systemctl daemon-reload'

pssh -i -h ~/k8s.hosts -- 'systemctl restart docker'

pssh -i -h ~/k8s.hosts -- 'systemctl restart kubelet'

ssh k8s-master kubectl get nodes

pssh -i -h ~/k8s.hosts -- 'ps aux | grep kube | grep CSI'

lolcat -a 03_enable_csi_outro.md
