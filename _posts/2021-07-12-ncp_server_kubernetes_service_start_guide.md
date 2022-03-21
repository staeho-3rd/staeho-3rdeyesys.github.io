---
date: 2021-07-12
last_modified_at: 2021-07-12
title: Kubernetes Service 클러스터 생성 및 제어 가이드
categories:
  - 1.compute
description: 네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드입니다
type: Document
set: server
order_number: 20
creator: franky
v2_link: /compute/ncloud_compute_kubernetes_service_start_guide.html
---

## 개요
네이버 클라우드 VPC 환경에서 Kubernetes(쿠버네티스) 서비스를 생성하고 제어하는 방법에 대해 소개합니다.

## 쿠버네티스란?
쿠버네티스(Kubernetes)는 배포, 스케일링, 그리고 컨테이너화된 애플리케이션의 관리를 자동화 해주는 오픈 소스 컨테이너 오케스트레이션 엔진으로 
구글에서 처음 개발하기 시작했으나 현재는 구글이 오픈소스 프로젝트로 공개한 상태입니다.

## 특징
쿠버네티스는 다음과 같은 특징이 있으며, 자세한 내용은 쿠버네티스 공식 페이지를 참고하시기 바랍니다.

<a href="https://kubernetes.io/ko/docs/home/" target="_blank" style="word-break:break-all;">https://kubernetes.io/ko/docs/home/</a>

- 서비스 디스커버리와 로드 밸런싱
- 스토리지 오케스트레이션
- 자동화된 롤아웃과 롤백
- 자동화된 빈 패킹(bin packing)
- 자동화된 복구(self-healing)
- 시크릿과 구성 관리




## 사전 준비
먼저 쿠버네티스 클러스터에 사용할 **전용 VPC와 Private Subnet, Load Balancer용 Subnet**이 필요합니다.

<img src="../../images/ncp_server_kebernetes_start_guide_07.jpg" alt="네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드 " style="width:770px;align:center">

{: .success }
- Private 대역(10.0.0.0/8,172.16.0.0/12,192.168.0.0/16) 내에서 /17~/26 범위의 Private Subnet, 로드밸런서 전용 Subnet이 필요합니다.<br/>
- Docker Bridge 대역의 충돌을 방지하기 위해 172.17.0.0/16 범위 내의 Private Subnet, 로드밸런서 전용Subnet은 선택할 수 없습니다.

## 클러스터 생성 

VPC와 Subnet이 준비되었다면, 다음으로 [Kubernetes Sevice] - [Cluster]에서 생성하기를 클릭합니다.

<img src="../../images/ncp_server_kebernetes_start_guide_09.jpg" alt="네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드 " style="width:500px;align:center">

생성하실 클러스터의 정보를 설정해줍니다.<br/>
(ACG는 자동으로 생성 됩니다.)

<img src="../../images/ncp_server_kebernetes_start_guide_01.jpg" alt="네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드 " style="width:770px;align:center">

{: .success }
현재 지원되고 있는 Kubenetes 버전은 [1.17.16], [1.18.17] 입니다.

클러스터에 생성되는 노드풀과 서버의 스펙 및 수를 지정해줍니다.

<img src="../../images/ncp_server_kebernetes_start_guide_02.jpg" alt="네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드 " style="width:770px;align:center">

{: .success }
현재 지원되고 있는 OS는 [ubuntu16.04], [ubuntu18.04] 입니다.

워커노드의 로그인키를 설정 합니다.

<img src="../../images/ncp_server_kebernetes_start_guide_03.jpg" alt="네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드 " style="width:770px;align:center">

설정 정보를 최종적으로 확인한 후 생성버튼을 클릭하여 클러스터를 생성합니다.

<img src="../../images/ncp_server_kebernetes_start_guide_04.jpg" alt="네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드 " style="width:770px;align:center">


## kubectl 설치

사용자 로컬PC에서 클러스터를 제어하기 위해 kubectl을 설치합니다.  
kubctl의 최신 릴리즈 버전은 아래  링크에서 다운받을 수 있습니다.

- kubctl v1.21.0 :  <a href="https://dl.k8s.io/release/v1.21.0/bin/windows/amd64/kubectl.exe" target="_self" style="word-break:break-all;">https://dl.k8s.io/release/v1.21.0/bin/windows/amd64/kubectl.exe</a>

또는 windows에서 cmd 창에서 curl 을 이용하해 다운로드 받을 수도 있습니다.
``` bash
> curl -LO https://dl.k8s.io/release/v1.21.0/bin/windows/amd64/kubectl.exe
```
<br />
kubectl의 버전 확인이 필요한 경우 다음 명령어를 사용하시면 됩니다.
``` bash
> kubectl version --client
```

<br />
다른 OS 환경에서 설치하는 방법은 아래 링크에서 확인할 수 있습니다.

- Linux kubectl 설치 : <a href="https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/" target="_self" style="word-break:break-all;">https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/</a>
- macOS kubectl 설치 : <a href="https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-macos/" target="_self" style="word-break:break-all;">https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-macos/</a>



## 접속-제어

클러스터를 제어하기 위해서는 네이버 클라우드 쿠버네티스 서비스에서 제공해주는 접속을 위한 인증정보가 있는 설정파일이 필요합니다.   
[설정파일] - [다운로드] 또는 [가이드 보기] - [설정파일 다운로드] 버튼을 클릭해 설정 파일을 다운로드 받습니다.

<img src="../../images/ncp_server_kebernetes_start_guide_05.jpg" alt="네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드 " style="width:770px;align:center">

kubectl을 실행해 Kubernetes에 접속하고 제어하는 방법은 [가이드 보기] 버튼 클릭하면 아래와 같이 자세히 확인할 수 있습니다.

<img src="../../images/ncp_server_kebernetes_start_guide_06.jpg" alt="네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드 " style="width:700px;align:center">


다운로드한 설정 파일경로를 %KUBE_CONFIG% 환경변수에 지정합니다.

``` bash
> SET KUBE_CONFIG=%USERPROFILE%\Downloads\kubeconfig-7349***-8***-4***-a***-99e***.yaml
```
<br />
이제 --kubeconfig 옵션을 사용하여 쿠버네티스의 클러스터를 제어할수 있습니다.

``` bash
> kubectl --kubeconfig %KUBE_CONFIG% get nodes
NAME                 STATUS   ROLES   AGE    VERSION
nks-pool-1865-w2zy   Ready    node    4d5h   v1.16.6
nks-pool-1865-w2zz   Ready    node    4d5h   v1.16.6
```

## 참고 URL
1. 쿠버네티스 사용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/vnks-nks-1-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/vnks-nks-1-1</a>
2. 클러스터 이용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/vnks-nks-1-2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/vnks-nks-1-2</a>
3. kubectl 설치 가이드
	- <a href="https://kubernetes.io/ko/docs/tasks/tools/" target="_blank" style="word-break:break-all;">https://kubernetes.io/ko/docs/tasks/tools/</a>
 