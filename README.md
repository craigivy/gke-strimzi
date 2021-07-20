# GKE Strimzi
This document covers installing Kafka on GKE using Strimzi.  For the most part if follows the instructions found [here](https://strimzi.io/docs/operators/latest/quickstart.html).  This project currently uses strimzi 0.24.0

## installation
The strimzi operator will be installed into the *kafka* namespace.
The kafka cluster will be installed in *my-kafka-project* namespace.


* Create workspaces for strimzi operator and kafka cluster
```
kubectl create ns kafka  
kubectl create ns my-kafka-project 
```

* Install operator
```
kubectl create -f strimzi/install/cluster-operator/ -n kafka
```

* Give permissions to operator to watch kafka project
```
kubectl create -f strimzi/install/cluster-operator/020-RoleBinding-strimzi-cluster-operator.yaml -n my-kafka-project
kubectl create -f strimzi/install/cluster-operator/031-RoleBinding-strimzi-cluster-operator-entity-operator-delegation.yaml -n my-kafka-project
```
* Create kafka clluster
```
kubectl create -n my-kafka-project -f kafka-gke.yaml
```
