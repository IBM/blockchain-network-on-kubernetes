## ** Work in Progress **
# Set up of Blockchain network using Kubernetes Cluster on IBM Cloud

Blockchain is a shared, immutable ledger for recording the history of transactions. Hyperledger Fabric is a blockchain framework implementation and one of the Hyperledger projects hosted by The Linux Foundation. For developing any blockchain (Hyperledger Fabric) application, you need to setup a Hyperledger Fabric network to develop, test and execute the application. Hyperledger Fabric network can be setup in multiple ways. 
* Setup Hyperledger Fabric network locally (http://hyperledger-fabric.readthedocs.io/en/release/build_network.html)
* Use [Blockchain as a service](https://console.bluemix.net/catalog/services/blockchain) hosted on [IBM Cloud](https://console.bluemix.net/). IBM Cloud provides you Blockchain as a service with a membership paid plan. 
* Setup Hyperledger Fabric network using [Kubernetes Cluster]((https://console.bluemix.net/containers-kubernetes/catalog/cluster)) on [IBM Cloud](https://console.bluemix.net/)

Hosting the Hyperledger Fabric network on IBM Cloud provides you additional benefits like multiple users can work on the same setup, the setup can be used for different blockchain applications, the setup can be reused and so on. This pattern demonstrates a way to setup your Hyperledger Fabric network using Kubernetes Cluster on IBM Cloud. Please note that the blockchain network setup on Kubernetes is good to use for demo scenarios, but for production you should use IBM Blockchain as a service hosted on IBM Cloud.

#### Kubernetes Cluster

[IBM Cloud Container Service](https://console.bluemix.net/containers-kubernetes/catalog/cluster) allows you to create a free cluster that comes with 2 CPUs, 4 GB memory, and 1 worker node. This is called _lite cluster_ and allows you to get familiar with and test Kubernetes capabilities. However they lack capabilities like persistent NFS file-based storage with volumes.

To setup your cluster for maximum availability and capacity, IBM Cloud allows you to create a fully customizable, production-ready cluster called _standard cluster_. _Standard clusters_ allow highly available cluster configurations such as a setup with two clusters that run in different regions, each with multiple worker nodes. Please see https://console.bluemix.net/docs/containers/cs_planning.html#cs_planning_cluster_config to review other options for highly available cluster configurations.

This pattern uses a _lite cluster_ provided by IBM Cloud and it can be used for proof-of-concept purpose. This pattern provides you the scripts to automate the process for setting up Hyperledger Fabric network using Kubernetes Cluster on IBM Cloud.

When the reader has completed this pattern, they will understand how to:

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
4. Deploy Hyperledger Fabric Network into Kubernetes Cluster
5. Test the deployed network
6. View the Kubernetes Dashboard

### 1. Create a Kubernetes Cluster on IBM Cloud

* Create a Kubernetes cluster with [IBM Cloud Container Service](https://console.bluemix.net/containers-kubernetes/catalog/cluster) using GUI. This pattern uses the _lite cluster_.

  Note: It can take up to 15 minutes for the worker node machine to be ordered and for the cluster to be set up and provisioned.


### 2. Setting up CLIs

* Install [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started). The prefix for running commands by using the Bluemix CLI is `bx`.

* Install [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/). The prefix for running commands by using the Kubernetes CLI is `kubectl`.

* Install the container service plugin using the following command.
  ```
  bx plugin install container-service -r Bluemix
  ```

### 3. Gain access to your cluster
  
  * Once your cluster is ready, open the access tab `IBM Cloud Dashboard -> <your cluster> -> Access` as shown in snapshot.
  
    ![](images/gain-access-to-cluster.png)
  
  * Perform the steps provided under the section `Gain access to your cluster`.

  * Verify that the kubectl commands run properly with your cluster by checking the Kubernetes CLI server version.
  
    ```
    $ kubectl version  --short
    Client Version: v1.9.2
    Server Version: v1.8.6-4+9c2a4c1ed1ee7e
    ```
    
### 4. Deploy Hyperledger Fabric network into Kubernetes clusters

#### Understand the network topology

This pattern provides a script which automatically provisions a sample Hyperledger Fabric network consisting of four organizations, each maintaining one peer node, and a 'solo' ordering service. Also, the script creates a channel named as `channel1`, joins all peers to the channel `channel1`, deploy and instantiate chaincode. The pattern will also help to drive execution of transactions against the deployed chaincode.

#### Copy Kubernetes configuration scripts

Clone or download the Kubernetes configuration scripts to your user home directory.
```
$ git clone https://github.com/IBM/blockchain-network-on-kubernetes.git
```

Navigate to the source directory
```
$ cd blockchain-network-on-kubernetes
$ ls
```
In the source directory, 
  * `configFiles` contains Kubernetes configuration files
  * `artifacts` contains the network configuration files
  * scripts to deploy and delete the network
  
#### Modify the Kubernetes configuration scripts

If there is any change in network topology, need to modify the configuration files(.yaml files) appropriately located in `artifacts` and `configFiles` directory. Everytime you do not need to make changes in all the files. It all depends on your network topology. For example, if you decide to increase/decrease the capacity of persistant volume then you need to modify `createVolume.yaml` only.  

#### Run the script to deploy your Hyperledger Fabric Network

Once you have completed the changes in configuration files, you are ready to deploy your network. Execute the script to setup your hyperledger fabric network.

```
$ ./setup_blockchainNetwork.sh
```

Note: Before running the script, please check your environment. You should able to run `kubectl commands` properly with your cluster as explained in step 3. 

#### Delete the network

If required, you can bring the network down as well. The script `deleteNetwork.sh` is provided to achieve that. The script will delete all your pods, jobs, deployments and so on from your kubernetes cluster.

```
$ ./deleteNetwork.sh
```

### 5. Test the deployed network

### 6. View the Kubernetes Dashboard

Obtain the token using the following command to authenticate to Kubernetes dashboard.

```
kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
```

Copy the token and launch your Kubernetes dashboard with the default port 8001.

```
kubectl proxy
```

Open the following URL in a web browser to see the Kubernetes dashboard. http://localhost:8001/ui
It will prompt for the token.

![](images/provide-token-for-dashboard.png)

Provide the token and `SIGN-IN`. In the Workloads tab, you can see the resources that you created.

![](images/kubernetes-dashboard.png)


## Troubleshooting

[See DEBUGGING.md.](DEBUGGING.md)

## License

[Apache 2.0](LICENSE)
