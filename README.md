## ** Work in Progress **
# Set up of Blockchain network using Kubernetes Cluster on IBM Cloud

For developing any blockchain (Hyperledger Fabric) application, you need to setup a Hyperledger Fabric network to develop, test and execute the application. Hyperledger Fabric network can be setup in multiple ways. 
* Setup Hyperledger Fabric network locally (http://hyperledger-fabric.readthedocs.io/en/release/build_network.html)
* Use Hyperledger Fabric network hosted on IBM Cloud (https://console.bluemix.net/catalog/services/blockchain)
* Setup Hyperledger Fabric network using Kubernetes Cluster on IBM Cloud.

This pattern demonstrates a way to setup your Hyperledger Fabric network using Kubernetes Cluster on IBM Cloud.

Hosting the Hyperledger Fabric network on [IBM Cloud](https://console.bluemix.net/) using [IBM Cloud Container Service](https://console.bluemix.net/containers-kubernetes/catalog/cluster) will provide you additional benefits like multiple users can work on the same setup, the setup can be used for different blockchain applications, the setup can be reused and so on.

This pattern provides you the scripts to automate the process for setting up Hyperledger Fabric network using Kubernetes Cluster on IBM Cloud.

## Flow

1. Log in to IBM Cloud CLI and initialize IBM Cloud Container Service plugin.
2. Set context for Kubernetes CLI by downloading Kubernetes configuration files and setting KUBECONFIG environment variable.
3. Run script `setup_blockchainnetwork.sh` to setup hyperledger fabric network.

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

* Once your cluster is ready, follow the steps provided `Dashboard -> Clusters -> Access` to gain access of your cluster.

# Steps

## 1. Setup IBM Cloud Container Service
### Create a Kubernetes Cluster

* Create a Kubernetes cluster with IBM Cloud Container Service using [GUI](https://console.bluemix.net/containers-kubernetes/catalog/cluster). IBM Cloud allows you to create a free cluster that comes with 2 CPUs, 4 GB memory, and 1 worker node. This is called _lite cluster_ and allows you to get familiar with and test Kubernetes capabilities. This pattern will be using this lite cluster.
Note: It can take up to 15 minutes for the worker node machine to be ordered and for the cluster to be set up and provisioned.


### Setting up CLIs

* Install [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started). The prefix for running commands by using the Bluemix CLI is `bx`.

* Install [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/). The prefix for running commands by using the Kubernetes CLI is `kubectl`.

* Install the container service plugin using the following command.
`bx plugin install container-service -r Bluemix`

### Gain access to your cluster
  Once your cluster is ready, follow the steps provided `IBM Cloud Dashboard -> <your cluster> -> Access` to gain access of your cluster.
  >>>snapshot

  * Verify that the kubectl commands run properly with your cluster by checking the Kubernetes CLI server version.
    ```
    $ kubectl version  --short
    Client Version: v1.7.4
    Server Version: v1.7.4-1+1540c973d4ff9d
    ```
## Step 2. Deploy Hyperledger Fabric network topology into Kubernetes clusters

### Copy Kubernetes configuration scripts
Clone or download the Kubernetes configuration scripts to your user home directory.
```
$ git clone https://github.com/IBM/blockchain-network-on-kubernetes.git
```

Navigate to the source directory
```
$ cd blockchain-network-on-kubernetes
$ ls
```



The network for which scripts are provided includes 4org and one peer in each org, one orderer, ca corresponding to each peer and orderer. If you want to change in this, accordingly yaml files need to modify.
Follow these steps to setup and run this code pattern. 
If there is any change in network topology, modify the files before execution.
To do hyperledger fabric network setup run the script provided.
./setup_blockchainnetwork.sh



# Troubleshooting

[See DEBUGGING.md.](DEBUGGING.md)

# License

[Apache 2.0](LICENSE)
