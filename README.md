## ** Work in Progress **
# Set up of Blockchain network using Kubernetes Cluster on IBM Cloud

For developing any blockchain (Hyperledger Fabric) application, you need to setup a blockchain network to develop, test and execute the application. Blockchain network can be setup in multiple ways. 
* Setup blockchain network locally (http://hyperledger-fabric.readthedocs.io/en/release/build_network.html)
* Use blockchain network hosted on IBM Cloud (https://console.bluemix.net/catalog/services/blockchain)
* Setup blockchain network using Kubernetes Cluster on IBM Cloud.

This pattern will demonstrate a way to setup your blockchain network using Kubernetes Cluster on IBM Cloud.

Hosting the blockchain network on [IBM Cloud](https://console.bluemix.net/) using [IBM Cloud Container Service](https://console.bluemix.net/containers-kubernetes/catalog/cluster) will provide you additional benefits like multiple users can work on the same setup, the setup can be used for different blockchain applications, the setup can be reused and so on.

This pattern will provide you the scripts to automate the process for setting up blockchain network using Kubernetes Cluster on IBM Cloud.

## Included components

* [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/): Hyperledger Fabric is a platform for distributed ledger solutions underpinned by a modular architecture delivering high degrees of confidentiality, resiliency, flexibility and scalability.

* [IBM Cloud Container Service](https://console.bluemix.net/containers-kubernetes/catalog/cluster): IBM Container Service enables the orchestration of intelligent scheduling, self-healing, and horizontal scaling.

## Featured technologies

* [Blockchain](https://en.wikipedia.org/wiki/Blockchain): A blockchain is a digitized, decentralized, public ledger of all transactions in a network.

* [Kubernetes Cluster](https://kubernetes.io/docs): In Kubernetes Engine, a container cluster consists of at least one cluster master and multiple worker machines called nodes. A container cluster is the foundation of Kubernetes Engine.

# Watch the Video

TODO

# Prerequisites

* Install [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started)

* Install [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

* Create a Kubernetes cluster with [IBM Cloud Container Service](https://console.bluemix.net/containers-kubernetes/catalog/cluster) to deploy in cloud.

* Once ypur cluster is ready, follow the steps provided `Dashboard -> Clusters -> Access` to gain access of your cluster.

# Steps

Follow these steps to setup and run this code pattern.

# Troubleshooting

[See DEBUGGING.md.](DEBUGGING.md)

# License

[Apache 2.0](LICENSE)
