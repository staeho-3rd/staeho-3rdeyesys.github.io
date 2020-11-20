# 서버 정지시 요금할인

## 개요
네이버 클라우드에서는 서버를 정지할 경우 일부 타입을 제외한 대부분의 서버에 대해 운영체제 설치를 위해 제공된 기본 디스크 요금만 청구가 되어 요금 할인이 됩니다.

## 추가 요금이 청구되는 서비스
공인 IP, 로드밸런서, 추가 디스크, Security Monitoring, 추가 Network Interface 등 서버에 연결된 다른 유료 서비스의 경우 서버가 정지되어도 정상 청구됩니다.


## 요금 할인 횟수와 서버 정지 기한
- 요금이 할인되는 서버의 경우 {==**1회 최대 90일, 12개월 누적 최대 180일**==}까지만 서버를 정지할 수 있습니다. 
- 서버 정기 가능 기한을 넘긴 서버는 고객에게 통보 후 서버를 반납하게 됩니다.
- 서버를 반납하게 될 때 서버에 저장된 데이터는 네이버 클라우드에서 30일간 직접 백업하여 보관 후 삭제하게 됩니다.

<img src="../../img/ncp_server_stop_price_discount.png" alt="서버 정기 기한과 요금할인" style="width:600px;align:center">

## 요금 할인이 적용되지 않는 서버 타입
일부 서버들은 서버를 정지해도 요금 할인이 되지 않고, 서버가 가동 중일 때와 동일한 요금이 청구됩니다.
할인이 적용되지 않는 서버 타입은 다음과 같습니다.

- Micro 서버
- High Memory 서버
- GPU 서버
- Virtual Dedicated Server
- Baremetal 서버


## 참고 URL
<a href="https://docs.ncloud.com/ko/compute/compute-1-1-v2.html" target="_blank">https://docs.ncloud.com/ko/compute/compute-1-1-v2.html</a>


!!! info "문서 최종 수정일 : 2020-11-13" 