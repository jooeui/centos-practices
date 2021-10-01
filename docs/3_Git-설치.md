# Git 설치

##### 1. 의존성 라이브러리

```sh
# /root 
$ yum -y install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel asciidoc xmlto
```



##### 2. 다운로드

```sh
$ wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz
```



##### 3. 압축풀기

```sh
$ tar xvfz git-2.9.5.tar.gz
```



##### 4. 소스 디렉토리

```sh
$ cd git-2.9.5
$ pwd
```



##### 5. configure

```sh
$ ./configure --prefix=/usr/local/douzone/git
```



##### 6. 빌드

```sh
$ make all
```



##### 7. 설치

```sh
$ make install
```



##### 8. 설정

- /etc/profile 코드 추가

```sh
$ vi /etc/profile
```

> ```
> # git
> export PATH=/usr/local/douzone/git/bin:$PATH
> ```

```sh
$ source /etc/profile 
```



##### 9. git 사용하기

```sh
# /root
$ mkdir centos-practices

$ cd centos-practices
$ git init
$ echo "# centos-practices" >> README.md
$ git add -A
$ git commit -m "first commit"
$ git branch -M main
$ git remote add origin https://github.com/jooeui/centos-practices.git
$ git push -u origin main
> Username for 'https://github.com': 깃이름입력
> Password for 'htpps://깃이름@github.com': token입력
```



**+) javastudy clone**

```sh
# /root
$ git clone https://github.com/jooeui/javastudy.git
$ cd javastudy
```

