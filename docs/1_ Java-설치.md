# Java 설치

##### 1.  JDK 설치

원래는 `wget`을 이용하여 설치하면 되지만 oracle에서 막아놨는지 안 된다 ... 
파일을 받아서 sftp를 이용하여 linux로 옮기자 ..!

```sh
[C:\]$ sftp webmaster@127.0.0.1
> put C:\Users\user\Downloads\jdk-8u291-linux-x64.tar.gz
```

> 로컬 환경에서 작업
> put 뒤에는 jdk 파일 경로 써주면 됨! (강의 자료 Java Programming 폴더에 파일 있음)

```sh
$ mv jdk-8u291-linux-x64.tar.gz /home/webmaster /root
$ tar xvfz jdk-8u291-linux-x64.tar.gz
$ mkdir /usr/local/douzone
$ mv jdk.1.8.0_291 /usr/local/douzone/jdk1.8
```



##### 2. 링크 파일 생성

```sh
$ ln -s /usr/local/douzone/jdk1.8 /usr/local/douzone/java
```



##### 3. 설정

- /etc/profile 코드 추가

```sh
$ vi /etc/profile
```

> **방법1**
>
> ```
> # java
> export PATH=$PATH:/usr/local/douzone/jdk1.8/bin
> export CLASSPATH=.:/usr/local/douzone/jdk1.8/lib/tools.jar
> ```
>
>
> **방법2**
>
> ```
> # java
> export JAVA_HOME=/usr/local/douzone/jdk1.8
> export PATH=$PATH:$JAVA_HOME/jdk1.8/bin
> export CLASSPATH=.:$JAVA_HOME/lib/tools.jar
> ```
>
> **방법3 - 이 방법을 사용!!**
>
> ```sh
> # java
> export JAVA_HOME=/usr/local/douzone/java
> export PATH=$PATH:$JAVA_HOME/jdk1.8/bin
> export CLASSPATH=.:$JAVA_HOME/lib/tools.jar
> ```

```sh
# 현재 shell 환경에 적용
$ source /etc/profile
```



##### 4. 확인

```sh
$ java -version
$ javac -version
```



## 실습

##### 1. 파일 생성

```sh
$ cd /root/dowork
$ vi HelloWorld.java
```

> **HelloWorld.java**
>
> ```java
> public class HelloWorld {
> 
>     public static void main(String[] args){
>         System.out.println("Hello World");
>     }
> }
> ```



##### 2. 컴파일

```sh
$ javac HelloWorld.java
```



##### 3. 실행

```sh
$ java HelloWorld
```

