# Tomcat 설치

##### 1. 다운로드

```sh
# /root
$ wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.71/bin/apache-tomcat-8.5.71.tar.gz
```

> tomcat - https://tomcat.apache.org/download-80.cgi



##### 2. 설치

```sh
$ tar xvfz apache-tomcat-8.5.71.tar.gz
$ mv apache-tomcat-8.5.71 /usr/local/douzone/tomcat8.5
```



##### 3. 링크 파일 생성

```sh
$ ln -s /usr/local/douzone/tomcat8.5 /usr/local/douzone/tomcat
```



#####  4. 포트 설정

```sh
$ vi /usr/local/douzone/tomcat/conf/server.xml
```

> `/` 입력 후 8080 검색
> 8080 -> 8088로 변경



##### 5. 실행

```sh
$ usr/local/douzone/tomcat/bin/catalina.sh start
# 실행확인
$ ps -ef | grep tomcat
$ ps -ef | grep java
```



##### 6. 브라우저로 접근하기

`http://localhost:8088` 또는 `http://127.0.0.1:8088`



##### 7. 중지

```sh
$ usr/local/douzone/tomcat/bin/catalina.sh stop
```

#####  

##### 8. 서비스 등록하기

- tomcat.service 파일 생성

```sh
 $ vi /usr/lib/systemd/system/tomcat.service
```

> ```
> [Unit]
> Description=tomcat8
> After=network.target syslog.target
> 
> [Service]
> Type=forking
> 
> Environment=JAVA_HOME=/usr/local/douzone/java
> User=root
> Group=root
> 
> ExecStart=/usr/local/douzone/tomcat/bin/startup.sh
> ExecStop=/usr/local/douzone/tomcat/bin/shutdown.sh
> 
> UMask=0007
> RestartSec=10
> Restart=always
> 
> [Install]
> WantedBy=multi-user.target
> ```

```sh
$ systemctl enable tomcat
```



##### 9. tomcat 서비스 실행/중지/재실행

```sh
$ systemctl start tomcat
$ systemctl stop tomcat
$ systemctl restart tomcat
```



##### 10. tomcat manager 설정

- tomcat-users.xml 설정

```sh
$ vi /usr/local/douzone/tomcat/conf/tomcat-users.xml
```

> ```xml
> <tomcat-users>
> ...
>     # 아래 코드 추가
>     <role rolename="manager"/>
>     <role rolename="manager-gui" />
>     <role rolename="manager-script" />
>     <role rolename="manager-jmx" />
>     <role rolename="manager-status" />
>     <role rolename="admin"/>
>     <user username="admin" password="manager" roles="admin,manager,managergui, manager-script, manager-jmx, manager-status"/>
> </tomcat-users>
> ```

- context.xml 설정

```sh
$ vi /usr/local/douzone/tomcat/webapps/manager/META-INF/context.xml
```

> ```xml
> # 주석
> <Context>
> ....
> </Context>
> 
> # 다음 내용 추가
> <Context antiResourceLocking="false" privileged="true" docBase="${catalina.home}/webapps/manager">
> 	<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
> </Context>
> ```



##### 11. tomcat 재시작

```sh
$ systemctl restart tomcat
# 또는
$ systemctl stop tomcat
$ ps -ef | grep tomcat	# 중지 확인
$ systemctl start tomcat
```



##### 12. 브라우저로 접근

`http://localhost:8088/manager` 또는 `http://127.0.0.1:8088/manager`

- 사용자 이름: admin
  비밀번호: manager



---



## Eclipse에서 tomcat 설정

##### 1. 프로젝트 생성

File > New > Dynamic Web Project > helloweb 생성



##### 2. jsp 파일 생성

helloweb - src - webapp 우클릭 > New > JSP File > index.jsp 생성

> New JSP File (html 5)



##### 3. Server Runtime Environment 추가

Window > Preferences > Server - Runtime Environments > Add > Apache Tomcat v8.0 선택 > Next > Browse... > C:\douzone2021-eui\apache-tomcat-8.5.71 폴더 선택 > Finish

>tomcat - https://tomcat.apache.org/download-80.cgi
>zip 다운로드 후 압축 해제! 해당 경로를 선택해주면 됨!!



##### 4. Server 생성

File > new > Other > Server 선택
Tomcat v8.5 Server 선택 > Next > helloweb 선택 > Add > Finish



##### 5. 실행

Window > Show View > Servers
Tomcat v8.5 Server at locathost 우클릭 > Start



#### CentOS에 helloweb 배치

##### 1. Export

helloweb 우클릭 > Export > War file > Browse... > C:\douzone2021-eui 경로에 helloweb.war 저장



##### 2. 배치

`http://localhost:8088/manager` 또는 `http://127.0.0.1:8088/manager` 접속
배치할 WAR 파일 - 파일 선택 > helloweb.war 선택 > 배치



##### 3. 확인

```sh
$ cd /usr/local/douzone/tomcat/webapps
$ ls -la
```

> helloweb.war 파일 있는 것을 확인할 수 있다!!
> centOS에서 tomcat이 실행되고 있었다면 war 파일이 풀려있음!

`http://localhost:8088/helloweb` 또는 `http://127.0.0.1:8088/helloweb` 접속

> 현재 index.jsp 파일에서 Titile만 Insert title here로 되어있고 body에는 아무것도 없기 때문에 웹 타이틀이  Insert title here로 보인다면 잘 된 것!!!!