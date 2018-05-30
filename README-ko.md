
# Kubernetes API를 사용하여 IBM 클라우드에 블록체인 네트워크 배포하기

블록 체인은 거래내역을 기록하기 위한 공유 불변의 원장입니다. Linux 재단의 Hyperledger Fabric은 IBM이 구현한 Permissioned 네트워크입니다. 블록체인의 활용 예제를 개발할 때 가장 먼저해야 할 일은 Hyperledger Fabric의 응용 프로그램 개발 및 배포 환경을 갖추는 것입니다. Hyperledger Fabric 네트워크를 구성하는 방법은 여러 가지가 있습니다.
* [Hyperledger Fabric network On-Premise](http://hyperledger-fabric.readthedocs.io/en/release-1.0/build_network.html)
* [IBM 클라우드](https://console.bluemix.net/)의 [블록체인 서비스](https://console.bluemix.net/catalog/services/blockchain). IBM 클라우드는 스타터 및 엔터프라이즈 멤버십 플랜 블록체인 서비스를 제공합니다.
* [IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster)에서 제공하는 [Kubernetes API](https://console.bluemix.net/containers-kubernetes/catalog/cluster)를 사용한 Hyperledger Fabric 네트워크

이번 코드 패턴은 **IBM 클라우드 컨테이너 서비스의 Kubernetes API를 활용한 Hyperledger Fabric**에서 비즈니스 네트워크를 설정하는 순서에 대한 것입니다.

Hyperledger Fabric 네트워크를 IBM 클라우드에서 호스팅 하면 여러 사용자가 같은 환경에서 작업이 가능하고, 환경을 다른 블록체인 애플리케이션에 재 사용하는 등 다양한 이점이 있습니다. Kubernetes의 블록체인 네트워크 설정은 데모 시나리오 목적으로는 충분하지만, 상업용으로서는 IBM 클라우드에서 호스팅 하는 블록체인 서비스를 추천합니다.

#### Kubernetes 클러스터

[IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster)는 Kubernetes 기능을 익히고 테스트할 수 있도록 2 개의 CPU, 4GB 메모리 및 1 개의 worker 노드로 이루어진 무료 클러스터 구성을 제공합니다. 그러나 NFS처럼 볼륨 기반의 파일 저장소 기능은 제공하지 않습니다.

IBM 클라우드는 클러스터의 최대 가용성과 기능성을 이끌어내기 위해 즉시 상업용으로 사용 가능하며 완벽한 커스터마이징을 지원하는 _Standard Cluster_ 를 제공합니다. _Standard Cluster_ 는 서로 다른 영역에서 실행되는 다중 작업노드를 포함한 클러스터 구성과 같은 고가용성 클러스터 설정이 가능합니다. 고가용성 클러스터 구성에 대한 다른 옵션을 검토하려면 https://console.bluemix.net/docs/containers/cs_planning.html#cs_planning_cluster_config를 참조하십시오.

이 패턴은 IBM 클라우드에서 제공하는 _free cluster_ 를 사용하며, 개념증명(proof-of-concept) 용도로 활용할 수 있습니다. 또한, IBM 클라우드의 Kubernetes API를 사용하여 Hyperledger Fabric 네트워크를 설정하는 프로세스를 자동화하는 스크립트를 제공합니다.


독자는 이 패턴을 완료하고 다음을 이해하게 될 것입니다:

* 네트워크 토폴로지에 따른 설정 파일 수정하기
* Kubernetes 클러스터에 Hyperledger Fabric 네트워크 배포하기

## 다음 순서를 따라하세요.

  ![](images/architecture.png)

1. IBM 클라우드 CLI에 접속하여 IBM 클라우드 컨테이너 서비스 플러그인을 초기화합니다.
2. CLI를 사용하여 Kubernetes 클러스터를에 대한 context를 변경하고 Kubernetes 설정 파일을 다운로드하세요. 설정 파일을 다운로드 한 후 KUBECONFIG 환경 변수를 설정하세요.
3. 스크립트를 실행하여 Kubernetes 클러스터에 Hyperledger Fabric 네트워크를 배포합니다.
4. Kubernetes의 대시보드로 이동하세요.

## 구성 요소

* [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/): Hyperledger Fabric은 고도의 기밀성, 탄력성, 유연성 및 확장성을 제공하는 모듈러 아키텍처 기반의 분산원장 술루션을 위한 플랫폼입니다.
* [IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster): IBM Container Service는 지능형 스케줄링, 자가치유 및 수평 확장의 조화로운 관리를 가능하게 합니다.

## 주요 기술

* [Blockchain](https://en.wikipedia.org/wiki/Blockchain): 블록체인은 네트워크의 모든 거래내역이 분산된 디지털 공개 원장입니다.
* [Kubernetes Cluster](https://kubernetes.io/docs): Kubernetes Engine에서 컨테이너 클러스터는 최소 하나의 마스터 클러스터와 노드라고 불리는 여러 작업 시스템으로 구성됩니다. 컨테이너 클러스터는 Kubernetes 엔진의 기반입니다.

## 비디오 참조

[![](http://img.youtube.com/vi/DFYk6XaMHc0/0.jpg)](https://youtu.be/DFYk6XaMHc0)

## Kubernetes의 개념 정의

* [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/) - Pod는 Kubernetes 클러스터에서 배포 가능한 최소 단위를 뜻하며, 단일로 처리해야 하는 여러 컨테이너를 그룹화하는 데 사용됩니다.
* [Kubernetes Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) - Job은 하나 이상의 Pod를 생성하고 지정된 수의 Pod가 성공적으로 종료하도록 합니다. Pod를 가 성공적으로 완료되면 Job은 이를 기록합니다.
* [Kubernetes Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) - Deployment는 저장장치, 서비스 및 어노테이션 등 어플리케이션을 실행하기 위해 필요한 기타 리소스와 컨테이너를 지정하는 Kubernetes 리소스입니다.
* [Kubernetes Services](https://kubernetes.io/docs/concepts/services-networking/service/) - Kubernetes service는 여러 Pod를 그룹화하고 클러스터 내 다른 서비스를 위해 개별 Pod의 IP 주소를 은폐하면서 네트워크 연결을 제공합니다.
* [Kubernetes Persistent Volumes (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) - Persistent Volume은 사용자가 NFS와 같은 파일 저장소를 요청하는 방법입니다.

## 순서

다음 순서를 따라 환경을 설정하고 코드를 실행합니다.

1. [IBM 클라우드에 Kubernetes 클러스터 생성](#1-IBM-클라우드에-Kubernetes-클러스터-생성)
2. [CLI 설치](#2-CLI-설치)
3. [Kubernetes 클러스터에 접근 권한 얻기](#3-Kubernetes-클러스터에-접근-권한-얻기)
4. [Kubernetes 클러스터에 Hyperledger Fabric 네터워크 배포](#4-Kubernetes-클러스터에-Hyperledger-Fabric-네터워크-배포)
5. [배포된 네트워크 테스트](#5-배포된-네트워크-테스트)
6. [Kubernetes 대시보드 확인](#6-Kubernetes-대시보드-확인)

### 1. IBM 클라우드에 Kubernetes 클러스터 생성

* Create a Kubernetes cluster with [IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster) using GUI. This pattern uses the _free cluster_.

* Create a Kubernetes cluster with [IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster) using GUI. This pattern uses the _free cluster_.

  ![](images/create-service.png)
  
  Note: It can take up to 15 minutes for the cluster to be set up and provisioned.


### 2. CLI 설치

* Install [IBM 클라우드 CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started). The prefix for running commands by using the Bluemix CLI is `bx`.

* Install [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/). The prefix for running commands by using the Kubernetes CLI is `kubectl`.

* Install the container service plugin using the following command.
  ```
  bx plugin install container-service -r Bluemix
  ```

### 3. Kubernetes 클러스터에 접근 권한 얻기
  
  Access the [IBM 클라우드 Dashboard](https://console.bluemix.net/dashboard/apps).  Choose the same cloud foundry org and cloud
  foundry space where cluster is created.
  
  * Check the status of your cluster `IBM 클라우드 Dashboard -> <your cluster> -> Worker Nodes`. If status is not `ready`, then
    you need to wait for some more time to proceed further.
    
    ![](images/cluster-status.png)
    
  * Once your cluster is ready, open the access tab `IBM 클라우드 Dashboard -> <your cluster> -> Access` as shown in snapshot.
  
    ![](images/gain-access-to-cluster.png)
  
  * Perform the steps provided under the section `Gain access to your cluster`.

  * Verify that the kubectl commands run properly with your cluster by checking the Kubernetes CLI server version.
  
    ```
    $ kubectl version  --short
    Client Version: v1.9.2
    Server Version: v1.8.6-4+9c2a4c1ed1ee7e
    ```
    
### 4. Kubernetes 클러스터에 Hyperledger Fabric 네터워크 배포

#### Understand the network topology

This pattern provides a script which automatically provisions a sample Hyperledger Fabric network consisting of four organizations, each maintaining one peer node, and a 'solo' ordering service. Also, the script creates a channel named as `channel1`, joins all peers to the channel `channel1`, install chaincode on all peers and instantiate chaincode on channel. The pattern also helps to drive execution of transactions against the deployed chaincode.

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
  * `*.sh` scripts to deploy and delete the network
  
#### Modify the Kubernetes configuration scripts

If there is any change in network topology, need to modify the configuration files (.yaml files) appropriately. The configuration files are located in `artifacts` and `configFiles` directory. For example, if you decide to increase/decrease the capacity of persistant volume then you need to modify `createVolume.yaml`.  

#### Run the script to deploy your Hyperledger Fabric Network

Once you have completed the changes (if any) in configuration files, you are ready to deploy your network. Execute the script to deploy your hyperledger fabric network.

  ```
  $ chmod +x setup_blockchainNetwork.sh
  $ ./setup_blockchainNetwork.sh
  ```

Note: Before running the script, please check your environment. You should able to run `kubectl commands` properly with your cluster as explained in step 3. 

#### Delete the network

If required, you can bring your hyperledger fabric network down using the script `deleteNetwork.sh`. This script will delete all your pods, jobs, deployments etc. from your Kubernetes cluster.

  ```
  $ chmod +x deleteNetwork.sh
  $ ./deleteNetwork.sh
  ```

### 5. 배포된 네트워크 테스트

After successful execution of the script `setup_blockchainNetwork.sh`, check the status of pods.

  ```
  $ kubectl get pods
  NAME                                    READY     STATUS    RESTARTS   AGE
  blockchain-ca-7848c48d64-2cxr5          1/1       Running   0          4m
  blockchain-orderer-596ccc458f-thdgn     1/1       Running   0          4m
  blockchain-org1peer1-747d6bdff4-4kzts   1/1       Running   0          4m
  blockchain-org2peer1-7794d9b8c5-sn2qf   1/1       Running   0          4m
  blockchain-org3peer1-59b6d99c45-dhtbp   1/1       Running   0          4m
  blockchain-org4peer1-6b6c99c45-wz9wm    1/1       Running   0          4m
  ```

As mentioned above, the script joins all peers on one channel `channel1`, install chaincode on all peers and instantiate chaincode on channel. It means we can execute an invoke/query command on any peer and the response should be same on all peers. Please note that in this pattern tls certs are disabled to avoid complexity. In this pattern, the CLI commands are used to test the network. For running a query against any peer, need to get into a bash shell of a peer, run the query and exit from the peer container.

Use the following command to get into a bash shell of a peer:

  ```
  $ kubectl exec -it <blockchain-org1peer1 pod name> bash
  ```

And the command to be used to exit from the peer container is:

  ```
  # exit
  ```

**Query**

Chaincode was instantiated with the values as `{ a: 100, b: 200 }`. Let’s query to `org1peer1` for the value of `a` to make sure the chaincode was properly instantiated.

  ![](images/first-query.png)

**Invoke**

Now let’s submit a request to `org2peer1` to move 20 from `a` to `b`. A new transaction will be generated and upon successful completion of transaction, state will get updated.

  ![](images/invoke.png)

**Query**

Let’s confirm that our previous invocation executed properly. We initialized the key `a` with a value of 100 and just removed 20 with our previous invocation. Therefore, a query against `a` should show 80 and a query against `b` should show 220. Now issue the query request to `org3peer1` and `org4peer1` as shown.

  ![](images/second-query.png)

  ![](images/third-query.png)

### 6. Kubernetes 대시보드 확인

Obtain the token using the following command to authenticate for Kubernetes dashboard.

  ```
  $ kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
  ```

Copy the token. Launch your Kubernetes dashboard with the default port 8001.

  ```
  $ kubectl proxy
  ```

Open the URL http://localhost:8001/ui in a web browser to see the Kubernetes dashboard. It will prompt for the authentication.

  ![](images/provide-token-for-dashboard.png)

Provide the token and `SIGN-IN`. In the Workloads tab, you can see the resources that was created through scripts.

  ![](images/kubernetes-dashboard.png)

The hyperledger fabric network is ready to use. You can start developing your blockchain applications using node sdk or hyperledger composer for this deployed network.

## Troubleshooting

[See DEBUGGING.md.](DEBUGGING.md)

## License

[Apache 2.0](LICENSE)
