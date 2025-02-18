---
date: 2020-12-28
last_modified_at: 2021-01-26
title: 설치형 DB서버와 관리형 Cloud DB 비교
categories:
  - 5.database
description: 네이버 클라우드 설치형 DB와 관리형 Cloud for DB 비교
type: Document
set: database
order_number: 1
v2_link: /database/ncloud_database_compare.html
---
## 개요
서버에 DB가 설치된 상태로 제공되는 설치형 DB서버와 Cloud 형태로 제공되는 관리형 DB서버는 어떤 특징과 차이점이 있는지 확인합니다.
더불어 비용 비교와 함께 각각의 DB서버를 어떤 경우에 사용하면 좋은지 예시를 통해 DB서버 선택에 도움을 드리고자 합니다.


## 설치형 DB  특징
- 저렴한 비용
- DB관련 아주 세부적인 부분까지 직접 설정 가능

## 관리형 Cloud DB 특징
- 빠르고 손쉬운 설치
- 네이버 클라우드에서 검증된 최적화 설정
- 자동으로 증가하는 데이터 스토리지 (MSSQL : 2TB까지, Mysql : 6000GB까지)
- 장애 발생시 자동 Fail-over를 통한 장애 최소화를 할 수 있는 탁월한 가용성 제공
- 읽기 부하 분산을 위한 읽기 전용 Slave 5개까지 지원
- 자동화된 DB 백업, 최대 30일까지 보관
- 성능 모니터링과 알람
- 원하는 시간을 선택하여 DB 자동 복원 (Mysql)
- 1분 단위의 쿼리 레벨 성능 분석을 지원 (MSSQL)

## 비용 전체 비교
- DB 서버 스펙 : Standard(2 vCPU, 4GB 메모리, 100GB 디스크)

| DB 구분 | 설치형 DB (서버 비용 포함) | 관리형 Cloud for DB  |
| :----: | :----: | :----: |
| mysql | 69,000원/월 | 115,200원/월 |
| MSSQL | 379,000원/월 | 614,880원/월 |


## 비용 비교 상세 (Mysql)

### 설치형
- 69,000원/월 : 서버 비용 + DB 무료

### 관리형 Cloud
-115,200원/월 : 160원(시간당) * 24시간(1일) * 30일(한달) 


## 비용 비교 상세 (MSSQL)

### 설치형
- 379,000원/월 : 69,000원(서버) + 20,000원(서버 Windows 라이선스) + 290,000원(MSSQL 라이선스)


### 관리형 Cloud
- 614,880원/월 : 854원 (시간당) * 24시간(1일) * 30일(한달)

{: .warning }
관리형 Cloud for MSSQL에서 HA (Principal-Mirror 구성) 구성을 할 경우, DBMS 라이선스 요금은 마스터/슬레이브 서버에만 적용됩니다.


## 설치형 DB서버를 사용하면 좋은 경우
- 사내에 DB전문가가 있을 경우
- 서비스에 최적화된 DB설정을 하고 싶은 경우
- 장애 시 자동 Fail-over가 굳이 필요하지 않은 경우
- DB백업을 원하는 방식으로 직접 하고 싶은 경우
- DB 사이즈가 일정 크기 이상으로 늘어나는 것을 원하지 않는 경우
- 서비스 안정성 보다 비용 절감이 더 중요한 경우


## 관리형 Cloud DB를 사용하면 좋은 경우
- 장애 시 자동 Fail-over를 통해 서비스 중지 시간을 최소로 하고 싶을 경우
- DB의 읽기 요청이 많아서 읽기 전용 DB를 마련했을 때 효과가 큰 경우
- DB백업과 디스크 용량 증설 등이 특별한 작업 없이 자동으로 진행되길 원하는 경우
- 비용보다 서비스 안정성이 더 중요한 경우
- DB전문가가 없는 경우



## 참고 URL
<a href="https://www.ncloud.com/product/database" target="_blank" style="word-break:break-all;">https://www.ncloud.com/product/database</a>
