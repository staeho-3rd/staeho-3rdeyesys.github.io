---
date: 2021-01-22
last_modified_at: 2021-06-11
title: mysql DB 자동백업 방법
categories:
  - 5.database
description: 네이버 클라우드 mysql DB 자동백업 방법
type: Document
set: database
order_number: 2
---
## 개요
매일 일정한 시간에 mysql DB를 자동으로 백업 받는 방법에 대해 정리해보았습니다.


## 백업 폴더 생성
루트에 /data_backup 폴더를 만들고 그 아래에 db 폴더를 생성합니다.
``` bash
~# mkdir /data_backup
~# mkdir /data_backup/db
```

## mysql DB 백업 스크립트 작성
``` bash
~# vi /bin/db_backup.sh

#!/bin/bash
DATE=$(date +%Y%m%d%H%M%S)
BACKUP_DIR=/data_backup/db/

# 전체 DB를 백업할 경우
mysqldump -u root -p디비패스워드 --all-databases > $BACKUP_DIR"backup_"$DATE.sql

# 특정 DB를 백업할 경우
# mysqldump -u root -p디비패스워드 --databases DB명  > $BACKUP_DIR"backup_"$DATE.sql



find $BACKUP_DIR -ctime +7 -exec rm -f {} \;

# DATE=$(date +%Y%m%d%H%M%S)는 백업할 파일명을 
# 202001224505 와 같은 형식으로 저장할 수 있게 날짜를 변수로 담습니다.  
# find $BACKUP_DIR -ctime +7 -exec rm -f {} \;  
# 여기서 -ctime +7은 7일이 지난 백업 파일을 찾아서 삭제하기 위한 코드입니다.  

# 추가로 분 단위로 설정하려고 할 때는 아래와 같이 
# -cmin +10 처럼 작성하면 10분이 지난 파일을 찾아서 삭제하게 됩니다.
# find $BACKUP_DIR -cmin +10 -exec rm -f {} \;
```

<br />
``` bash
# 백업 스크립트에 실행 권한을 부여합니다.
~# chmod 755 /bin/db_backup.sh
```

## 스케쥴링을 위한 crontab 설정
``` bash
~# crontab -e

# 매일 새벽 6시에 백업이 진행됩니다.
00 06 * * * /bin/db_backup.sh
```
<br />
#### 그 외 시간 설정 방법
``` bash
# 30분 마다 실행
*/30 * * * * /bin/db_backup.sh

# 매주 일요일 새벽 6시에 실행
0 06 * * 0 /bin/db_backup.sh

# 매월 1일 새벽 6시에 실행
0 06 1 * * /bin/db_backup.sh

# 매년 12월 31일 새벽 6시에 실행
0 06 31 12 * /bin/db_backup.sh
```


## 참고 URL
<a href="https://www.ncloud.com/product/database" target="_blank" style="word-break:break-all;">https://www.ncloud.com/product/database</a>
