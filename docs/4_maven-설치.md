# maven 설치

##### 1. 다운로드

```sh
# /root
$ wget https://mirror.navercorp.com/apache/maven/maven-3/3.8.1/binaries/apache-maven-3.8.1-bin.tar.gz
```



##### 2. 압축 풀기

```sh
$ tar xvfz apache-maven-3.8.1-bin.tar.gz
```



##### 3. 설치

```sh
$ mv apache-maven-3.8.1 /usr/local/douzone/maven3.8
$ cd /usr/local/douzone/
$ ls -la
$ ln -s
$ ln -s /usr/local/douzone/maven3.8 /usr/local/douzone/maven
```



##### 4. 설정

```sh
$ vi /etc/profile
```

> ```
> # maven
> export PATH=/usr/local/douzone/maven/bin:$PATH
> ```

```sh
$ source /etc/profile
```



##### 5. 설치 확인

```sh
$ mvn --version
```



---



**javastudy 프로젝트 실행시키기 - network로 실습**

maven을 사용하기 위해서 finalName을 설정해줘야 함!

##### 1. javastudy/network/pom.xml 수정

```xml
<project>
	...
	<build>
  		<finalName>network</finalName>
  	</build>
</project>
```



##### 2. git push



##### 3. 가져오기

```sh
# /root
$ cd javastudy
$ git pull

# clone 안 했다면
$ git clone https://github.com/jooeui/javastudy.git
$ cd javastudy
```



##### 4. 실행

```sh
$ mvn clean package
```

