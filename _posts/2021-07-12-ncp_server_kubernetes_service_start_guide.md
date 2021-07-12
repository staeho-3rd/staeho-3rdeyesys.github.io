---
date: 2021-07-12
update: 2021-07-12
title: Kubernetes Service 클러스터 생성 및 제어 가이드
categories:
  - 1.compute
description: 네이버 클라우드 Kubernetes Service 클러스터 생성 및 제어 가이드입니다
type: Document
set: server
order_number: 20
creator: franky
---

## 개요
네이버 클라우드에서 제공하는 Kubernetes(쿠버네티스) 서비스를 생성하고 제어하는 방법에 대해 소개합니다.

## 쿠버네티스란?
쿠버네티스는 배포, 스케일링, 그리고 컨테이너화된 애플리케이션의 관리를 자동화 해주는 오픈 소스 컨테이너 오케스트레이션 엔진이라고 정의되어 있습니다.  
구글에서 처음 개발하기 시작했으나 현재는 구글이 오픈소스 프로젝트로 공개한 상태입니다.

## 특징
쿠버네티스는 다음과 같은 특징이 있으며, 자세한 내용은 쿠버네티스 공식 페이지를 참고하시기 바랍니다.

- 서비스 디스커버리와 로드 밸런싱
- 스토리지 오케스트레이션
- 자동화된 롤아웃과 롤백
- 자동화된 빈 패킹(bin packing)
- 자동화된 복구(self-healing)
- 시크릿과 구성 관리

<a href="https://kubernetes.io/ko/docs/home/" target="_blank" style="word-break:break-all;">https://kubernetes.io/ko/docs/home/</a>


## NKS 클러스터 생성 

__선행조건__

클러스터에 사용할 전용VPC 와 Private, LB전용 Subnet 이 필요합니다.

>-Private 대역(10.0.0.0/8,172.16.0.0/12,192.168.0.0/16) 내에서 /17~/26 범위의 Private Subnet, 로드밸런서 전용 Subnet이 필요합니다.<br/>
-Docker Bridge 대역의 충돌을 방지하기 위해 172.17.0.0/16 범위 내의 Private Subnet, 로드밸런서 전용Subnet은 선택할 수 없습니다.


![](nks_9.png)


kubernetes Sevice탭에 들어간뒤 생성하기를 클릭합니다.


![](nks_1.png)

생성하실 클러스터의 정보를 설정해줍니다.<br/>
(ACG는 자동으로 생성 됩니다.)
>현재 지원되고 있는 Kubenetes 버전은 <1.17.16>, <1.18.17> 입니다.

![](nks_2.png)

클러스터에 생성되는 노드풀과 서버의 스펙 및 수를 지정해줍니다.

>현재 지원되고 있는 OS는 <ubuntu16.04> <ubuntu18.04> 입니다.

![](nks_3.png)

워커노드의 로그인키를 설정 합니다.

![](nks_4.png)

설정 정보를 최종적으로 확인한 후 생성버튼을 클릭하여 클러스터를 생성합니다.

## kubectl 설치

사용자 로컬 머신에서 클러스터를 제어하기 위해 kubectl을 설치합니다.

windows에서 cmd 창을 열어 kubctl의 최신 릴리즈 버전을 다운로드 합니다.

[최신릴리즈 v1.21.0](https://dl.k8s.io/release/v1.21.0/bin/windows/amd64/kubectl.exe)

또는 curl 을 이용하여 다운로드 받습니다.
```
curl -LO https://dl.k8s.io/v1.21.0/bin/windows/amd64/kubectl.exe.sha256
```

kubectl 의 버전을 확인합니다.
```
kubectl version --client
```
다른OS의 설치방법도 필요하시면 아래의 링크가 있습니다.

[Linux kubectl 설치](https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/)</br>
[macOS kubectl 설치](https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-macos/)



## NKS 접속하여 제어하기 (windows)

클러스터를 제어하기 위해서는 NKS에서 제공해주는 접속을 위한 인증정보가 있는 설정파일을 다운로드 합니다.

![](nks_5.png)

가이드 보기 버튼을 누른 뒤 


![](nks_6.png)


NKS에서 제공해주는 설정 파일을 다운로드 받습니다. 



그 후 다운로드한 파일경로로 %KUBE_CONFIG% 환경변수를 지정합니다.

예시)
```
> SET KUBE_CONFIG=%USERPROFILE%\Downloads\kubeconfig-734929a7-8a13-49a2-a2ca-99e7d5d9661e.yaml
```

이제 --kubeconfig 옵션을 사용하여 NKS의 클러스터를 제어할수 있습니다.

예시)
```
> kubectl --kubeconfig %KUBE_CONFIG% get nodes
NAME                 STATUS   ROLES   AGE    VERSION
nks-pool-1865-w2zy   Ready    node    4d5h   v1.16.6
nks-pool-1865-w2zz   Ready    node    4d5h   v1.16.6
```


## 참조 



## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-4-1-v2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-4-1-v2.html</a>

https://guide.ncloud-docs.com/docs/vnks-nks-1-1<br/>
https://guide.ncloud-docs.com/docs/vnks-nks-1-2<br/>
https://kubernetes.io/ko/docs/tasks/tools/

> 문서 최종 수정일 : 2021-07-12