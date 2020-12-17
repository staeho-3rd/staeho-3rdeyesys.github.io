# CLOUDWATCH LOG 수집 매뉴얼

## 사전 작업

### IAM 계정생성 및 권한 부여

cloudwatch log수집을 위해서는 서버 작업과 별도로 IAM 계정생성과 권한할당 작업을 선 진행하여야하며 작업이 완료된 후 계정의 액세스키, 비밀키를 발급 받는다.

 1. 계정생성
   계정생성- 계정이름 입력- 액서스 유형 Programmatic access 선택
   
 2. 권한설정
 
   기존 정책 직접 연결 선택 - CloudWatchAgentServerPolicy정책 추가
 3. 키확인
 
   액세스 키 확인 및 비밀키 확인



## 서버 작업

### 설치 작업

1. awslogs 설치는 다음 명령어를 통해 설치
   yum install -y awslogs
2. aws configure를 통한 IAM 키 입력

   aws configure 명령어 입력 - 액세스키 요구 및입력 - 비밀키 요구및 입력(위 IAM계정의 키 입력)

### 설정 관련

- /etc/awslogs/awscli.conf

  region = ap-northeast-2 추가 설정없이 리전만 로그 수집을 위한 리전으로 변경

- /etc/awslogs/awslog.conf

  해당 컨피그 파일의 제일 하단에 로그 수집 대상 로그를 아래의 형식으로 작성

  [/var/log/messages] -- 수집로그 경로와 파일 지정

  datetime_format = %b %d %H:%M:%S (로그의 데이터 포맷 지정)

  [^%Y]: year  1970, 1988, 2001, 2019
  [^%H]: Hour (24 -hour clock)
  [^%I]: Hour (12-hour clock)
  [^%p]: AM or PM
  [^%M]: minute 00, 01 ... ,59
  [^%S]: Second 00, 01, ... ,59
  [^%f]: Microsecond 000000, 000001, ... ,999999
  [^B]: 로캘 전체 이름 형태의 월. January, February, ..., December
  [^b]: 로캘 약어 형태의 월. Jan, Feb, ..., Dec



  file = /var/log/messages (로그 파일 위치)

  buffer_duration = 5000 (로그 이벤트를 일괄 처리하는 기간을 지정합니다. 최소값은 5000ms이고, 기본값은 5000ms입니다.)

  log_stream_name = {instance_id} (대상로그 스트림 이름 지정{instance_id}, {hostname}, {ip_address})

  initial_position = start_of_file

  log_group_name = /var/log/messages(cloudwatch 로그 그룹네임 지정 )

  time_zone=LOCAL

  multi_line_start_pattern ={datetime_format} (로그 줄 단위 기준점. datetime_fomat 멀티라인 처리)

- /var/log/awslogs.log

  awslogs서비스 실행 후 발생되는 로그.

### 실행 및 자동실행 등록

- 실행 명령어 service awslogs start
- 리붓시 자동실행 등록 chkconfig awslogs on (amazon linux2 인 경우 systemctl enable awslogsd.service)

## cloudwatch 설정

정상적으로 서버의 로그가 수집된다면 cloudwatch - log항목에서 서버에서 지정한 로그 그룹네임이 보이며 이를 클릭시 서버 인스턴스 ID별로 로그 수집되는 내역 확인 가능



수집된 데이터는 대시보드를 통해 데이터를 보여주는기능은 바로 가능하나 이를 가지고 그래프를 통한 시각화를 할수 없어 지표를 통한 그래프를 생성하여야함.

1. cloudwatch -로그- 로그그룹중 필터기능을 쓸 지표선택 -지표 필터 클릭

2. 지표 필터 추가 버튼 클릭

3.  필터링 하고자하는 값을 넣고 [정규식지원]패턴 테스트 후 지표할당 클릭

4. 이후 지표 네임스페이스는 로그 그룹 네임 중 추출하고자하는 지표영역 지표이름은 해당 지표를 선택

5. cloudwatch - 지표 선택 - 모든 지표에서 로그 그룹 필터를 적용한 로그 그룹의 incomingLogEvents선택

6. 그래프로 표시된 지표 탭 선택 - 필터가 적용된 그래프가 나오며 그래프의 작업 대시보드 추가를 선택하여

   대시보드로 그래프 등록



## 오류 대처 법

로그 수집이 되지 않고 awslogs.log파일에 아래와 같이 로그 가 찍히는 경우 처리 방안

> reason: timestamp is more than 2 hours in future.

1.  /var/lib/awslogs/agent-state를 삭제

   해당 방법으로 처리시 수집되는 로그가 처음부터 다시 로그가 수집됩

2.  sqlite3 명령어를 통해 agent-state 오류 처리

   1. sudo sqlite3 /var/lib/awslogs/agent-state
   2. select * from stream_state; 통해 문제가 되는 소스ID확인
   3. select * from push_state where k="확인된 소스ID";
   4. update push_state set v='... insert new value here ...' where k='7675f84405fcb8fe5b6bb14eaa0c4bfd';
   5. service awslogs restart

## 참고 URL

<https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/logs/AgentReference.html>

<https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/logs/QuickStartEC2Instance.html>

https://stackoverflow.com/questions/40604940/cloudwatch-logs-acting-weird





1. 계정 생성

   IAM 계정생성 - 사용자 이름을 정해준 뒤 액세스 유형 Programmatic access 선택

2. 권한 부여

   기존 정책 직접 연결 선택-  정책 중 CloudWatchAgentServerPolicy 선택

3. 계정생성 후 액세스키, 비밀키 확인
