# Kubernetes API를 사용하여 IBM 클라우드에 블록체인 네트워크 배포하기

*Read this in other languages: [English](README.md)*

블록체인은 거래 내역의 무결성이 보장된 공유 원장입니다. 리눅스 재단의 Hyperledger Fabric은 IBM이 구현한 Permissioned 네트워크입니다. 블록체인의 활용 예제를 개발할 때 가장 먼저해야 할 일은 Hyperledger Fabric의 응용 프로그램 개발 및 배포 환경을 갖추는 것입니다. Hyperledger Fabric 네트워크를 구성하는 방법은 여러 가지가 있습니다.
* [Hyperledger Fabric 네트워크 온프레미스](http://hyperledger-fabric.readthedocs.io/en/release-1.0/build_network.html)에 구성하기
* [IBM 클라우드](https://console.bluemix.net/)의 [블록체인 서비스](https://console.bluemix.net/catalog/services/blockchain). IBM 클라우드는 스타터 및 엔터프라이즈 멤버십 플랜으로 블록체인 서비스를 활용하기
* [IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster)에서 제공하는 [Kubernetes API](https://console.bluemix.net/containers-kubernetes/catalog/cluster)를 사용한 Hyperledger Fabric 네트워크를 활용하기 

이번 코드 패턴에서는 **Hyperledger Fabric에서 IBM 클라우드 컨테이너 서비스의 Kubernetes API를 활용**하여 비즈니스 네트워크를 설정하는 방법을 알아봅니다.

Hyperledger Fabric 네트워크를 IBM 클라우드에서 호스팅하면 여러 사용자가 같은 환경에서 작업이 가능하고, 다른 블록체인 애플리케이션에 환경을 재사용하는 등 다양한 이점이 있습니다. Kubernetes의 블록체인 네트워크 설정은 데모 시나리오 목적으로는 충분하지만, 비즈니스 용도로는 IBM 클라우드에서 호스팅하는 블록체인 서비스를 추천합니다.

#### Kubernetes 클러스터

[IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster)는 Kubernetes 기능을 익히고 테스트할 수 있도록 2개의 CPU, 4GB 메모리 및 1 개의 worker 노드로 이루어진 무료 클러스터 구성을 제공합니다. 그러나 NFS처럼 볼륨 기반의 파일 저장소 기능은 제공하지 않습니다.

IBM 클라우드는 클러스터의 최대 가용성과 기능성을 이끌어내기 위해 즉시 비즈니스용으로 사용 가능하며 완벽한 커스터마이징을 지원하는 _Standard Cluster_ 를 제공합니다. _Standard Cluster_ 는 서로 다른 영역에서 실행되는 다중 작업노드를 포함한 클러스터 구성과 같은 고가용성 클러스터 설정이 가능합니다. 고가용성 클러스터 구성에 대한 다른 옵션을 검토하려면 https://console.bluemix.net/docs/containers/cs_planning.html#cs_planning_cluster_config 를 참조하세요.

이 코드 패턴은 IBM 클라우드에서 제공하는 _free cluster_ 를 사용하며, PoC(proof-of-concept) 용도로 활용할 수 있습니다. 또한, IBM 클라우드의 Kubernetes API를 사용하여 Hyperledger Fabric 네트워크를 설정하는 프로세스를 자동화하는 스크립트를 제공합니다.

이 패턴을 완료하시면 다음을 배울 수 있습니다:

* 네트워크 토폴로지에 따른 설정 파일 수정하기
* Kubernetes 클러스터에 Hyperledger Fabric 네트워크 배포하기

## 다음 순서를 따라하세요.

  ![](images/architecture.png)

1. IBM 클라우드 CLI에 접속하여 IBM 클라우드 컨테이너 서비스 플러그인을 초기화합니다.
2. CLI를 사용하여 Kubernetes 클러스터에 대한 context를 변경하고 Kubernetes 설정 파일을 다운로드하세요. 설정 파일을 다운로드한 후 KUBECONFIG 환경 변수를 설정하세요.
3. 스크립트를 실행하여 Kubernetes 클러스터에 Hyperledger Fabric 네트워크를 배포합니다.
4. Kubernetes의 대시보드로 이동하세요.

## 구성 요소

* [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/): Hyperledger Fabric은 고도의 기밀성, 탄력성, 유연성 및 확장성을 제공하는 모듈러 아키텍처 기반의 분산원장 솔루션을 위한 플랫폼입니다.
* [IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster): IBM 컨테이너 서비스는 지능형 스케줄링, 자가 치유 및 수평 확장을 조화롭게 관리합니다.

## 주요 기술

* [블록체인](https://en.wikipedia.org/wiki/Blockchain): 블록체인은 네트워크의 모든 거래 내역이 분산된 디지털 공개 원장입니다.
* [Kubernetes Cluster](https://kubernetes.io/docs): Kubernetes Engine에서 컨테이너 클러스터는 최소 하나의 마스터 클러스터와 노드라고 불리는 여러 작업 시스템으로 구성됩니다. 컨테이너 클러스터는 Kubernetes 엔진의 기반입니다.

## 참조 동영상

[![](http://img.youtube.com/vi/DFYk6XaMHc0/0.jpg)](https://youtu.be/DFYk6XaMHc0)

## Kubernetes의 개념 정의

* [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/) - Pod는 Kubernetes 클러스터에서 배포 가능한 최소 단위를 뜻하며, 단일로 처리해야 하는 여러 컨테이너를 그룹화하는 데 사용됩니다.
* [Kubernetes Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) - Job은 하나 이상의 Pod를 생성하고 지정된 수의 Pod가 성공적으로 종료하도록 합니다. Pod를 가 성공적으로 완료되면 Job은 이를 기록합니다.
* [Kubernetes Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) - Deployment는 저장장치, 서비스 및 어노테이션 등 애플리케이션을 실행하기 위해 필요한 기타 리소스와 컨테이너를 지정하는 Kubernetes 리소스입니다.
* [Kubernetes Services](https://kubernetes.io/docs/concepts/services-networking/service/) - Kubernetes service는 여러 Pod를 그룹화하고 클러스터 내 다른 서비스를 위해 개별 Pod의 IP 주소를 은폐하면서 네트워크 연결을 제공합니다.
* [Kubernetes Persistent Volumes (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) - Persistent Volume은 사용자가 NFS와 같은 파일 저장소를 요청하는 방법입니다.

## 순서

다음 순서를 따라 환경을 설정하고 코드를 실행합니다.

1. [IBM 클라우드에 Kubernetes 클러스터 생성](#1-ibm-클라우드에-kubernetes-클러스터-생성)
2. [CLI 설치](#2-cli-설치)
3. [Kubernetes 클러스터에 접근 권한 얻기](#3-kubernetes-클러스터에-접근-권한-얻기)
4. [Kubernetes 클러스터에 Hyperledger Fabric 네트워크 배포](#4-kubernetes-클러스터에-hyperledger-fabric-네트워크-배포)
5. [배포된 네트워크 테스트](#5-배포된-네트워크-테스트)
6. [Kubernetes 대시보드 확인](#6-kubernetes-대시보드-확인)

### 1. IBM 클라우드에 Kubernetes 클러스터 생성

* [IBM 클라우드 컨테이너 서비스](https://console.bluemix.net/containers-kubernetes/catalog/cluster) GUI를 사용하여 Kubernetes 클러스터를 생성합니다. 이 패턴에서는 _free cluster_ 을 사용합니다.

  ![](images/create-service.png)
  
  주의: 클러스터를 설정하고 준비하는 데에는 최대 15분이 걸릴 수 있습니다.


### 2. CLI 설치

* [IBM 클라우드 CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started)를 설치하세요. Bluemix CLI로 명령을 실행하려면 `bx` 를 사용합니다.

* [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/)를 설치하세요. Kubernetes CLI로 명령을 실행하려면 `kubectl` 을 사용합니다.

* 다음 명령을 실행하여 컨테이너 서비스 플러그인을 설치하세요.
  ```
  bx plugin install container-service -r Bluemix
  ```

### 3. Kubernetes 클러스터에 접근 권한 얻기
  
  [IBM 클라우드 대시보드](https://console.bluemix.net/dashboard/apps)에서 클러스터가 생성된 동일한 클라우드 파운드리 조직 및 클라우드 파운드리 공간을 선택하세요.
  
  * 클러스터의 상태를 확인합니다. `IBM Cloud Dashboard -> <your cluster> -> Worker Nodes` 상태가 `ready`로 바뀔 때까지 계속 기다립니다.
    
    ![](images/cluster-status.png)
    
  * 클러스터가 준비되면 아래 스냅샷에 보이는대로 Access 탭을 엽니다. `IBM Cloud Dashboard -> <your cluster> -> Access`
  
    ![](images/gain-access-to-cluster.png)
  
  * `Gain access to your cluster`에 제공된 가이드를 따라 순서대로 진행합니다.

  * Kubernetes CLI 서버 버전을 확인하여 kubectl 명령이 클러스터에서 제대로 실행되는지 확인합니다.
  
    ```
    $ kubectl version  --short
    Client Version: v1.9.2
    Server Version: v1.8.6-4+9c2a4c1ed1ee7e
    ```
    
### 4. Kubernetes 클러스터에 Hyperledger Fabric 네트워크 배포

#### 네트워크 토폴로지 이해하기

이 패턴은 각 1개의 피어와 '솔로' 주문 서비스로 구성된, 총 4개의 조직을 포함한 Hyperledger Fabric 네트워크 샘플을 자동으로 준비하는 스크립트를 제공합니다. 스크립트는 또한 `channel1`이란 이름의 채널을 생성하고 모든 피어를 이 채널로 연결합니다. 그 다음, 체인코드를 모든 피어에게 설치하여 채널에 인스턴스화합니다. 이 패턴은 배포된 체인코드에 대해 트랜잭션 실행이 이루어지도록 도움을 줍니다.

#### Kubernetes 설정 스크립트 복사하기

Kubernetes 설정 스크립트를 사용자의 홈 디렉토리에 복사 또는 다운로드합니다.
  ```
  $ git clone https://github.com/IBM/blockchain-network-on-kubernetes.git
  ```

소스 디렉토리로 이동합니다.
  ```
  $ cd blockchain-network-on-kubernetes
  $ ls
  ```
소스 디렉토리 안에, 
  * `configFiles` 디렉토리는 Kubernetes 설정 파일을 포함하고 있습니다.
  * `artifacts` 디렉토리는 네트워크 설정 파일을 포함하고 있습니다.
  * `*.sh` 파일은 네트워크를 배포 또는 삭제하는 스크립트입니다.
  
#### Kubernetes 설정 스크립트 수정하기

네트워크 토폴로지가 변경되면 구성 파일 (.yaml 파일)을 적절하게 수정해야 합니다. 설정 파일은 `artifacts`와 `configFiles` 디렉토리에 있습니다. 예를 들어, 볼륨의 용량을 늘리거나 줄이려면 `createVolume.yaml` 파일을 수정해야 합니다. 

#### 스크립트를 실행하여 Hyperledger Fabric 네트워크 배포하기

설정 파일이 완성되면 네트워크를 배포할 준비가 된 것입니다. 스크립트를 실행하여 Hyperledger Fabric 네트워크를 배포하세요.

  ```
  $ chmod +x setup_blockchainNetwork.sh
  $ ./setup_blockchainNetwork.sh
  ```

주의: 스크립트를 실행하기 전에 실행환경을 먼저 확인하세요. 3번째 순서에서 설명한 대로 클러스터에서 kubectl 명령을 올바르게 실행할 수 있어야 합니다.

#### 네트워크 삭제하기

필요한 경우, `deleteNetwork.sh` 스크립트 파일을 실행하여 네트워크를 종료하세요. 이 스크립트는 Kubernetes 클러스터에서 모든 Pod, Job, Deployment 등을 삭제합니다.

  ```
  $ chmod +x deleteNetwork.sh
  $ ./deleteNetwork.sh
  ```

### 5. 배포된 네트워크 테스트

`setup_blockchainNetwork.sh` 스크립트 파일 실행이 성공하면 Pod의 상태를 확인합니다.

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

위에서 언급했듯이, 이 스크립트는 모든 피어를 하나의 채널 `channel1`에 연결하고, 그 피어에 체인코드를 설치하여 채널에서 체인코드를 인스턴스화합니다. 이는 모든 피어에 대해 invoke / query 명령을 실행할 수 있음을 의미하며 응답은 모든 피어에서 동일해야 합니다. 이 코드 패턴에서는 손쉬운 진행을 위해 TLS 인증서를 배제하였음을 유의하시기 바랍니다. CLI 명령은 네트워크를 테스트하는데 사용됩니다. 모든 피어에 대해 쿼리를 실행하려면 피어의 bash 셸에서 쿼리를 실행하고 피어 컨테이너에서 나와야 합니다.

피어의 bash 쉘에 들어가려면 다음 명령을 사용하세요:
  ```
  $ kubectl exec -it <blockchain-org1peer1 pod name> bash
  ```

피어 컨테이너에서 나가는 명령은 다음과 같습니다:
  ```
  # exit
  ```

**쿼리**

체인 코드는 `{a : 100, b : 200}` 값으로 인스턴스화 되었습니다. 체인코드가 제대로 인스턴스화 되었는지 확인하기 위해 `org1peer1`에 쿼리를 요청하여 `a`의 값을 확인해 봅니다.

  ![](images/first-query.png)

**반영**

이제 `org2peer1`에 `a`에서 `b`로 20을 이동하라는 요청을 보냅니다. 새로운 트랜잭션이 생성되고 트랜잭션이 성공적으로 완료되면 상태가 업데이트됩니다.

  ![](images/invoke.png)

**쿼리**

우리의 이전 변경이 제대로 반영되었는지 확인합시다. 우리는 `a`키를 100으로 초기화 하고, 이전 호출로 20을 제거했습니다. 그러므로 `a`에 대한 결과는 80이어야 하며 `b`의 결과는 220이어야 합니다. 이제 아래 그림과 같이 `org3peer1`와 `org4peer1`에 쿼리 요청을 보냅니다.

  ![](images/second-query.png)

  ![](images/third-query.png)

### 6. Kubernetes 대시보드 확인

Kubernetes 대시보드 인증을 위해 다음 명령을 사용하여 토큰을 얻습니다.

  ```
  $ kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
  ```

토큰을 복사하고 기본 포트를 8001로 설정하여 Kubernetes 대시보드를 시작합니다.

  ```
  $ kubectl proxy
  ```

웹브라우져에서 http://localhost:8001/ui URL을 열어 Kubernetes 대시보드로 이동하면 인증을 요구하는 프롬프트가 표시됩니다.

  ![](images/provide-token-for-dashboard.png)

토큰과 `SIGN-IN`을 입력합니다. Workloads탭에서 스크립트를 통해 생성된 리소스를 확인할 수 있습니다.

  ![](images/kubernetes-dashboard.png)

이제 Hyperledger Fabric 네트워크가 준비되었습니다. 이 배포된 네트워크에 노드 SDK 또는 Hyperledger Composer를 사용하여 블록체인 애플리케이션을 개발해보세요.

## 문제해결

[DEBUGGING.md 참조](DEBUGGING.md)

## 라이센스

[Apache 2.0](LICENSE)
