# Let's play with snapshots!
kubectl create -f ./statefulset/snap.yaml

kubectl get volumesnapshots.snapshot.storage.k8s.io

kubectl get volumesnapshotcontents.snapshot.storage.k8s.io

kubectl get pvc
/opt/emc/scaleio/mdm/bin/cli --mdm_ip 10.247.48.81 --query_volume_tree --volume_name ${VOLNAME}
