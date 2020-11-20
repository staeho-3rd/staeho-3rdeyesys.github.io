# Auto Scaling 서비스 제한사항

## 개요
모든 클라우드 서비스의 핵심 중의 하나가 Auto Scaling이라고 할 수 있습니다.
네이버 클라우드도 예외가 아닌데, Auto Scaling을 설정할 때 몇가지 제한사항이 있어서 정리해보았습니다.

## 스펙 및 서비스 환경 제한 사항
서버 스펙이나 서비스 환경과 관련한 제한 사항은 다음과 같습니다.

- 총 디스크 사이즈 150GB 이하 서버만 가능
- Windows OS는 Windows 2012. 2016만 지원
- Micro 서버는 불가
- High Memory 서버는 불가(추후 개선 예정)
- Local Disk 기반 서버는 불가
- Global Internet Service 영역 내의 서비스 불가

따라서 서버타입 기준으로  {==**Auto Scaling 설정이 가능한 서버타입은 Compact, Standard 2가지 뿐**==}입니다.

<img src="../../img/ncp_server_autoscaling_limit.png" alt="Auto Scaling 서비스 제한사항" style="width:600px;align:center">

## OS 서버 이미지 제한 사항
centos-7.8-64, ubuntu-18.04 이 2가지 OS 이미지는 개인 회원은 KR-1 1세대 서버에서 생성이 불가능한 이미지입니다. 2세대 서버를 선택하시거나 KR-2에서 생성해야 합니다.

## 설정 제한 사항
다음으로 Auto Scaling 설정을 할 때 생성 가능한 최대 서버 수 등의 설정 제한 사항은 다음과 같습니다.

- 고객별 생성 가능한 Auto Scaling Group 최대 수: 100
- 고객별 생성 가능한 Launch Configuration 최대 수: 100
- Auto Scaling Group당 생성 가능한 스케줄(Scheduled Action) 최대 수: 100
- Auto Scaling Group당 생성 가능한 Scaling Policy 최대 수: 10
- Auto Scaling Group당 생성 가능한 최대 서버 수: 30대
- Auto Scaling Group당 연결 가능한 Load Balancer 최대 수 : 10

!!! important "계정당 생성 가능한 최대 서버 대수"
	네이버 클라우드 플랫폼에서 한 **계정당 생성할 수 있는 최대 서버 수 기본 50대**입니다. 서버 수 한도를 조정하려면 고객지원으로 문의해야 합니다.

## 참고 URL
<a href="https://docs.ncloud.com/ko/compute/autoscaling/autoscaling_overview.html" target="_blank">https://docs.ncloud.com/ko/compute/autoscaling/autoscaling_overview.html</a>


!!! info "문서 최종 수정일 : 2020-11-19" 