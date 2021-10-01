# Jenkins 설치

##### 1. 설치

```sh
# /root
$ wget https://get.jenkins.io/war-stable/2.303.1/jenkins.war
```



##### 2. 이동

```sh
$ ps -ef | grep tomcat	# 실행되어있는지 먼저 확인
$ mv jenkins.war /usr/local/douzone/tomcat/webapps/
```



##### 3. 확인

```sh
$ cd /usr/local/douzone/tomcat/webapps/
$ ls -la
```

> jenkins.war 파일이 자동으로 풀려있는 것을 알 수 있음! -> tomcat이 해줌



##### 4. 브라우저로 접근 

`http://localhost:8088/jenkins`



##### 5. Administrator password

```sh
$ cd /root/.jenkins/secrets
$ cat initialAdminPassword
```

> 나오는 password를 복사하여 브라우저 Administrator password 입력란에 붙여넣기
> 그 후 추천 플러그인 설치!!! Install suggested plugins !!!



##### 6. Create First Admin User 정보 입력 후 Save

> 사용자 이름은 jenkins로 통일합시당..



##### 7. Jenkins 관리 설정

Jenkins 관리 > Global Tool Configuration

> **JDK**
> Add JDK > Install automatically 체크 해제
> Name : jdk1.8 / JAVA_HOME : /usr/local/douzone/jdk1.8
>
> **Git** 
> Add Git > Install automatically 체크 해제
> Name : git / Path to Git executable : /usr/local/douzone/git/bin/git



##### 8. 프로젝트 생성

새로운 Item > Freestyle project

> Enter an item name : javastudy

소스 코드 관리

> **Git**
> Repository URL : https://github.com/jooeui/javastudy.git

빌드 환경

> **Build**
> Add build step > Invoke top-level Maven targets
> Maven Version : maven 선택 / Goals : clean package



##### 9. Build

Build Now 선택

> 에러 난다면 javastudy 프로젝트 내에 에러가 발생하는 코드가 있는지 확인!
>  :star:에러 발생하는 파일이 없어야 빌드 가능!!!!!!!!!:star: