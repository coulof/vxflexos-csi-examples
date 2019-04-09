lolcat -a 20_postgres_intro.md 

ssh k8s-master kubectl get nodes

# first let's get the examples
curl -LO https://github.com/coulof/vxflexos-csi-examples/archive/master.zip
unzip master.zip
cd vxflexos-csi-examples-master/crunchy-postgres

# make sure vxflexos driver runs & storage classes are present
kubectl get storageclasses.storage.k8s.io
kubectl describe storageclasses.storage.k8s.io vxflexos

kubectl get pods -n vxflexos -o wide

# check the configuration of the stateful set
vi vxflexos-csi-examples/crunchy-postgres/statefulset/templates/set.yaml

# install the 2 instances of postgres
helm install statefulset --name statefulset

watch helm status statefulset
# we can now validate our storage from k8s PoV
kubectl get pvc
kubectl get pv

# and from VxFlexOS
/opt/emc/scaleio/mdm/bin/cli --mdm_ip 10.247.48.81 --login --username admin --password 'Dangerous@123' --force_login

/opt/emc/scaleio/mdm/bin/cli --mdm_ip 10.247.48.81 --query_all_volumes | grep k8s-demo

# see our 3 services
kubectl get services -o wide

# one for the master, pgset-primary
# one for the replica, pgset-replica
# one for the alias to the primary, pgset

kubectl exec -ti pgset-0 -- bash

psql -h pgset -U postgres

\l

SELECT CURRENT_USER usr, :'HOST' host, inet_server_port() port;

pgbench -h pgset-primary -U postgres -i -s 10 userdb


psql -h pgset -U postgres

\c userdb

select count(*) from pgbench_accounts;

psql -h pgset-replica -U postgres userdb

# let's try to insert data on the replica
pgbench -h pgset-replica -U postgres -i -s 10 userdb

# note this is read only
kubectl get pods -o wide
kubectl cordon ${NODE}
kubectl get nodes 
kubectl get pods -o wide

kubectl delete pod pgset-0
kubectl uncordon ${NODE}

kubectl get pods -o wide
# The pod restarted
kubectl exec -ti pgset-0 -- bash

psql -h pgset -U postgres userdb

\dt
-- Our data is still present
select count(*) from pgbench_accounts;


# let's scale our setup
kubectl scale statefulset --replicas=3 pgset

kubectl get pods

kubectl get pvc
/opt/emc/scaleio/mdm/bin/cli --mdm_ip 10.247.48.81 --query_all_volumes | grep k8s-demo 

kubectl exec -ti pgset-2 -- bash
psql -h pgset-2 -U postgres userdb

\dt
-- Our data is already there
select count(*) from pgbench_accounts;
