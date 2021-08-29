# GKE Strimzi
This document covers installing Kafka on GKE using Strimzi.  For the most part if follows the instructions found [here](https://strimzi.io/docs/operators/latest/quickstart.html).  This project currently uses strimzi 0.24.0

Assumptions:  You have a GKE cluster and elevated permissions to complete the steps below.

Note: This is a simple demo and does not include securing the resources. 

## Installation
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

* Create a kafka topic
```
kubectl create -n my-kafka-project -f kafka-topic.yaml
```

## Using kafka cluster
* Install kafka on a local machine and add the bin directory to your path. kafka can be downloaded at http://kafka.apache.org/

* Get the ip and port of the cluster

```
kubectl get service my-cluster-kafka-extplain-bootstrap -n my-kafka-project -o=jsonpath='{.status.loadBalancer.ingress[0].ip}{"\n"}'

kubectl get service my-cluster-kafka-extplain-bootstrap -n my-kafka-project -o=jsonpath='{.spec.ports[0].port}{"\n"}'
```

* Replace ```<ip>``` and ```<port>``` with the responses from the previous kubectl commands and run the following commands. 
```
kafka-console-producer.sh --broker-list <ip>:<port> --topic my-topic


kafka-console-consumer.sh --bootstrap-server <ip>:<port> --topic my-topic --from-beginning
```

* Create a kafka user
```
kubectl create -n my-kafka-project -f strimzi/examples/user/kafka-user.yaml
```





