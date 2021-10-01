# MariaDB 설치

##### 1. 의존 라이브러리 설치

```sh
$ yum -y install gcc gcc-c++ libtermcap-devel gdbm-devel zlib* libxml* freetype* libpng* flex gmp ncurses-devel cmake.x86_64 libaio

# iconv 소스 컴파일 설치
$ wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.16.tar.gz
$ tar xvfz libiconv-1.16.tar.gz
$ ./configure --prefix=/usr/local
$ make
$ make install
```

>
> iconv - https://www.gnu.org/software/libiconv/#TOCdownloading



##### 2. 소스 다운로드

```sh
# /root
$ wget https://downloads.mariadb.org/interstitial/mariadb-10.1.48/source/mariadb-10.1.48.tar.gz/from/https%3A//archive.mariadb.org/
```

> mariaDB - https://downloads.mariadb.org/mariadb/+releases/
> 다른 버전은 왜 에러 뜨는지는 모르겠으나.. 에러뜨니까 10.1.48 사용..!



##### 3. 압축 풀기

```sh
$ mv index.html mariadb-10.1.48.tar.gz
$ tar xvfz mariadb-10.1.48.tar.gz
$ cd mariadb-10.1.48
```



##### 4. 빌드 환경 설정

```sh
# MySQL 대표 포트번호는 3306
$ cmake -DCMAKE_INSTALL_PREFIX=/usr/local/douzone/mariadb -DMYSQL_USER=mysql -DMYSQL_TCP_PORT=3306 -DMYSQL_DATADIR=/usr/local/douzone/mariadb/data -DMYSQL_UNIX_ADDR=/usr/local/douzone/mariadb/tmp/mariadb.sock -DINSTALL_SYSCONFDIR=/usr/local/douzone/mariadb/etc -DINSTALL_SYSCONF2DIR=/usr/local/douzone/mariadb/etc/my.cnf.d -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DWITH_ARIA_STORAGE_ENGINE=1 -DWITH_XTRADB_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_FEDERATEDX_STORAGE_ENGINE=1 -DWITH_PERFSCHEMA_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DWITH_SSL=bundled -DWITH_ZLIB=system
```



##### 5. 빌드

```sh
$ make
```



##### 6. 설치

```sh
$ make install
# 설치 확인
$ cd /usr/local/douzone
$ ls -la
```



##### 7. 계정 생성

```sh
# /root
$ groupadd mysql
$ useradd -M -g mysql mysql
```



##### 8. 인스톨 디렉토리 /mariadb 소유자 변경

```sh
$ chown -R mysql:mysql /usr/local/douzone/mariadb
# 변경 확인
$ cd /usr/local/douzone
$ ls -la
```



##### 9. 설정파일 위치 변경

```sh
$ cp -R /usr/local/douzone/mariadb/etc/my.cnf.d /etc
```



##### 10. 기본(관리) 데이터베이스(MySQL) 생성

```SH
$ /usr/local/douzone/mariadb/scripts/mysql_install_db --user=mysql --basedir=/usr/local/douzone/mariadb --defaults-file=/usr/local/douzone/mariadb/etc/my.cnf --datadir=/usr/local/douzone/mariadb/data
```



##### 11. 서버 구동

```sh
$ /usr/local/douzone/mariadb/bin/mysqld_safe &
```



##### 12. root 패스워드 설정

```sh
$ /usr/local/douzone/mariadb/bin/mysqladmin -u root password
> New password: 설정할 비밀번호 입력
> Confirm new password: 비밀번호 확인
```



##### 13. 데이터베이스 접속 테스트

```sh
$ /usr/local/douzone/mariadb/bin/mysql -u root -p
```

> `-u`를 생략하면 현재 작업을 하는 사용자로 생각한다!
> 	-> 현재 작업자는 root이므로 root로 접속, webmaster로 접속하면 webmaster로!

```mariadb
MariaDB[none]> show databases;
```



##### 14. PATH 설정

- /etc/profile 코드 추가

```sh
$ vi /etc/profile
```

```
# mysql 
export PATH=/usr/local/douzone/mariadb/bin:$PATH
```

```sh
$ source /etc/profile
```



##### 15. 서비스(데몬, Daemon) 등록/시작/중지

```sh
$ cp /usr/local/douzone/mariadb/support-files/mysql.server /etc/init.d/mariadb
$ chkconfig --list	# 서비스 나열
$ ststemctl enable mariadb	# 등록
$ ststemctl start mariadb	# 시작
$ ststemctl stop mariadb	# 종료
```



**+) 포트포워딩**

VirtualBox
장치 > 네트워크 > 네트워크 설정 > 고급 > 포트포워딩 > 추가

> 호스트 IP: 127.0.0.1
> 호스트 포트: 3306
> 게스트 IP: 10.0.2.15 (쉘에서 ifconfig하여 나오는 걸로 해야 함!!)
> 게스트 포트: 3306

**테스트**

```sh
[C:\~]$ telnet 127.0.0.1 3306
```



---



## MariaDB 데이터베이스 사용자 생성 권한 부여

##### 1. 데이터베이스 접속

```sh
$ mysql -u root -p
> 비밀번호 입력
```

> 현재 root로 접근하고 있기 때문에 `-u root` 생략 가능



##### 2. database 생성

```mariadb
MariaDB[none]> create database webdb;
```

#####  

##### 3-1. user 생성

```mariadb
MariaDB[none]> create user 'webdb'@'localhost' identified by 'webdb';
```

> '아이디'@'localhost' identified by '비밀번호';
> 내부에서만 접속 가능!



##### 4. 권한 부여

```mariadb
MariaDB[none]> grant all privileges on webdb.* to 'webdb'@'localhost';
```

> webdb에서 모든 권한 부여



##### 5. 새 변경사항 적용

```mariadb
MariaDB[none]> flush privieges;
```



##### 6. 테스트

```sh
$ mysql -u webdb -D webdb -p
> 패스워드 입력(webdb)
```

---

##### 3-2. user 생성

```mariadb
MariaDB[none]> create user 'webdb'@'10.0.2.2' identified by 'webdb';
```

> '아이디'@'게이트웨이' identified by '비밀번호';
> 쉘에서 `netstat -r` 입력하면 확인 가능



##### 4. 권한 부여

```mariadb
MariaDB[none]> grant all privileges on webdb.* to 'webdb'@'10.0.2.2';
```



##### 5. 새 변경사항 적용

```mariadb
MariaDB[none]> flush privieges;
```



##### 6. 테스트

Windows의 Workbench에서 테스트

New Connection(+ 선택)

> Connection Name: webdb
> Port: 3306 (빌드 환경 설정 단계 `DMYSQL_TCP_PORT=포트번호`에서 작성한 포트번호)
> Username: webdb
> Password - Store in Vault ... : webdb(mariadb user생성에서 작성한 비밀번호)
> Default Schema: webdb

