
kubectl create namespace vxflexos

echo -n 'admin' | base64
echo -n 'Dangerous@123' | base64

kubectl create -f secret.yaml

sh verify.kubernetes
sh install.vxflexos

kubectl get storageclasses.storage.k8s.io

kubectl create namespace test