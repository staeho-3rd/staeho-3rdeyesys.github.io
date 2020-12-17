# [Ubuntu] http 접속 시에 https로 강제 리다이렉트 시키는 방법 - Apache

## 개요
웹사이트 SSL 인증서를 설치하고 https 접속을 유도할 때 http로 접속하면 https로 강제로 리다이렉트 시키는 방법을 사용하는 경우가 많습니다.  
웹페이지 소스에서 http 접속 여부를 판단해서 redirect 시키는 방법 등 여러가지 있을 수 있는데 여기서는 Apache 설정으로 쉽게 할 수 있는 방법을 소개합니다.  

SSL 인증서가 설치되어 있다는 가정하에 우선 Linux Ubuntu에서 설정하는 방법을 확인해보겠습니다.

## Rewrite 모듈 설치
혹시 Apache에 이미 mod_rewrite 가 로드 되어 있다면 설치하지 않아도 됩니다.

``` bash
root@test-lamp:~# a2enmod rewrite
```

## Apache conf 파일 수정

**/etc/apache2/sites-enabled/000-default.conf** 의 Vritual host에 다음 코드를 추가하고 Apache를 재시작하면 됩니다.
``` bash
RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R,L]
```

000-default.conf 파일에 실제로 적용하면 다음과 비슷한 모습이 되겠습니다.
``` bash
<VirtualHost *:80>
	DocumentRoot "/ncp/data/www/"
	ServerName www.test.com

	RewriteEngine On
	RewriteCond %{HTTPS} !on
	RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R,L]
</VirtualHost>
```


## Apache 재시작
``` bash
root@test-lamp:~# systemctl restart apache2
```
이렇게 재시작하고 http로 접속을 해보면 https로 전환되는 것을 확인할 수 있습니다.


## Apache 재시작 오류 - SSLEngine
위 내용처럼 아파치 재시작 명령어를 입력하면 끝입니다만, 재시작이 되지 않고 오류 메시지가 뜨는 경우가 있습니다.
``` bash
root@test-lamp:~# systemctl restart apache2
Job for apache2.service failed because the control process exited with error code. 
See "systemctl status apache2.service" and "journalctl -xe" for details.
```

오류 메시지에 나온 것 처럼 상세 내용을 확인해봅니다.
``` bash
root@test-lamp:~# systemctl status apache2.service
```
상세 오류 메시지 중에서 핵심 내용만 살펴보면 다음과 같습니다.
``` bash
Dec 04 16:09:44 test-lamp apachectl[11924]: AH00526: Syntax error on line 36 of /etc/apache2/sites-enabled/000-default.conf:
Dec 04 16:09:44 test-lamp apachectl[11924]: Invalid command 'SSLEngine', perhaps misspelled or defined by a module not included in the server configuration
```

위 오류 메시지에 나온 /etc/apache2/sites-enabled/000-default.conf 파일을 찾아가서 SSLEngine을 확인해보면 다음과 같습니다.
``` bash
<VirtualHost *:443>
    ServerName www.test.com
    ServerAdmin web@test.com

    {==SSLEngine on==}
    SSLCertificateFile /etc/******/server.crt
    SSLCertificateKeyFile /etc/******/server.key

    ###중략###
</VirtualHost>
```

확인해보면 SSLEngine on 명령어가 제대로 동작하지 못해서 생긴 문제라는 것을 알 수 있습니다.     
즉, SSL 모듈이 제대로 활성화 되지 않았기에 활성화 해주면 문제가 해결됩니다.  
(나중에 살펴보면 socache_shmcb 모듈이 활성화되지 않았기 때문임을 알 수 있습니다)

## SSL 엔진 모듈 활성화
``` bash
root@test-lamp:~# a2enmod ssl
```

그리고 나서 다시 Apache를 재시작 하면 됩니다.
``` bash
root@test-lamp:~# systemctl restart apache2
```

이제 http로 접속하셔서 https로 전환되는지 확인해보시기 바랍니다.


!!! info "문서 최종 수정일 : 2020-12-07"