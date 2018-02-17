## ** Work in Progress **
# Set up of Blockchain network using Kubernetes Cluster on IBM Cloud

For developing any blockchain (Hyperledger Fabric) application, you need to setup a Hyperledger Fabric network to develop, test and execute the application. Hyperledger Fabric network can be setup in multiple ways. 
* Setup Hyperledger Fabric network locally (http://hyperledger-fabric.readthedocs.io/en/release/build_network.html)
* Use [Blockchain as a service](https://console.bluemix.net/catalog/services/blockchain) hosted on [IBM Cloud](https://console.bluemix.net/). IBM Cloud provides you Blockchain as a service with a membership paid plan. 
* Setup Hyperledger Fabric network using [Kubernetes Cluster]((https://console.bluemix.net/containers-kubernetes/catalog/cluster)) on [IBM Cloud](https://console.bluemix.net/)

Hosting the Hyperledger Fabric network on IBM Cloud provides you additional benefits like multiple users can work on the same setup, the setup can be used for different blockchain applications, the setup can be reused and so on. This pattern demonstrates a way to setup your Hyperledger Fabric network using Kubernetes Cluster on IBM Cloud.

IBM Cloud allows you to create a free cluster that comes with 2 CPUs, 4 GB memory, and 1 worker node. This is called _lite cluster_ and allows you to get familiar with and test Kubernetes capabilities. However they lack capabilities like persistent NFS file-based storage with volumes.

To setup your cluster for maximum availability and capacity, IBM Cloud allows you to create a fully customizable, production-ready cluster called _standard cluster_. _Standard clusters_ allow highly available cluster configurations such as a setup with two clusters that run in different regions, each with multiple worker nodes. Please see https://console.bluemix.net/docs/containers/cs_planning.html#cs_planning_cluster_config to review other options for highly available cluster configurations.

This pattern uses a _lite cluster_ provided by IBM Cloud and it can be used for proof-of-concept purpose. This pattern provides you the scripts to automate the process for setting up Hyperledger Fabric network using Kubernetes Cluster on IBM Cloud.

When the reader has completed this pattern they will understand how to:

* modify configuration files according to their network topology
* set up their own hyperledger fabric network on Kubernetes cluster

## Flow

1. Log in to IBM Cloud CLI and initialize IBM Cloud Container Service plugin.
2. Set context for Kubernetes CLI by downloading Kubernetes configuration files and setting KUBECONFIG environment variable.
3. Run script `setup_blockchainNetwork.sh` to setup hyperledger fabric network.

## Included components

* [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/): Hyperledger Fabric is a platform for distributed ledger solutions underpinned by a modular architecture delivering high degrees of confidentiality, resiliency, flexibility and scalability.

* [IBM Cloud Container Service](https://console.bluemix.net/containers-kubernetes/catalog/cluster): IBM Container Service enables the orchestration of intelligent scheduling, self-healing, and horizontal scaling.

## Featured technologies

* [Blockchain](https://en.wikipedia.org/wiki/Blockchain): A blockchain is a digitized, decentralized, public ledger of all transactions in a network.

* [Kubernetes Cluster](https://kubernetes.io/docs): In Kubernetes Engine, a container cluster consists of at least one cluster master and multiple worker machines called nodes. A container cluster is the foundation of Kubernetes Engine.

## Watch the Video

TODO

## Kubernetes Concepts Used
* [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/) - Pods represent the smallest deployable units in a Kubernetes cluster and are used to group containers that must be treated as a single unit.
* [Kubernetes Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) - A job creates one or more pods and ensures that a specified number of them successfully terminate. As pods successfully complete, the job tracks the successful completions.
* [Kubernetes Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) - A deployment is a Kubernetes resource where you specify your containers and other Kubernetes resources that are required to run your app, such as persistent storage, services, or annotations.
* [Kubernetes Services](https://kubernetes.io/docs/concepts/services-networking/service/) - A Kubernetes service groups a set of pods and provides network connection to these pods for other services in the cluster without exposing the actual private IP address of each pod.
* [Kubernetes Persistent Volumes (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) - PersistentVolumes are a way for users to *claim* durable storage such as NFS file storage.

## Steps

1. Create a Kubernetes Cluster on IBM Cloud
2. Setting up CLIs
3. Gain access to your Kubernetes Cluster
4. Deploy Hyperledger Fabric Network Topology into Kubernetes Cluster

## 1. Setup IBM Cloud Container Service
### 1. Create a Kubernetes Cluster

* Create a Kubernetes cluster with [IBM Cloud Container Service](https://console.bluemix.net/containers-kubernetes/catalog/cluster) using GUI. This pattern uses the _lite cluster_.

Note: It can take up to 15 minutes for the worker node machine to be ordered and for the cluster to be set up and provisioned.


### 2. Setting up CLIs

* Install [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started). The prefix for running commands by using the Bluemix CLI is `bx`.

* Install [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/). The prefix for running commands by using the Kubernetes CLI is `kubectl`.

* Install the container service plugin using the following command.
`bx plugin install container-service -r Bluemix`

### 3. Gain access to your cluster
  Once your cluster is ready, follow the steps provided `IBM Cloud Dashboard -> <your cluster> -> Access` to gain access of your cluster.
  >>>snapshot

  * Verify that the kubectl commands run properly with your cluster by checking the Kubernetes CLI server version.
    ```
    $ kubectl version  --short
    Client Version: v1.7.4
    Server Version: v1.7.4-1+1540c973d4ff9d
    ```
## 4. Deploy Hyperledger Fabric network topology into Kubernetes clusters

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
