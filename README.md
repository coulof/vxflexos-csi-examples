# CSI VxFlexOS Examples
This repository contains a collection of sample configuration to use VxFlexOS as a storage class in Kubernetes.

The examples are usually simple forks of standard configurations but tuned to use VxFlexOS storage backend.

## [crunchy-postgres](crunchy-postgres)
That directory contains few  helm charts to deploy a PostgreSQL cluster in configuration.

It relies on [CrunchyData](https://crunchydata.github.io/crunchy-containers/stable/) distribution which provides native High Availability configuration

### [statefulstate](crunchy-postgres/statefulstate)
The modifications made to the [original helm chart](https://github.com/CrunchyData/crunchy-containers/tree/master/examples/helm/statefulstate) are :
* vxflexos as [default storage class](crunchy-postgres/statefulset/values.yaml#L27)
* A new [service](crunchy-postgres/statefulstate/service.yaml) NodePort (TCP 31432) to ease connection to the DB from external world
* A bigger default pvc

## [demos](demos/README.md)
In this directory you will find example configurations and videos on the usage of the driver.