# Codyssey_project

포트 매핑 및 접속 증거
    브라우저 접속 화면(또는 curl 응답)을 기술 문서에 첨부한다.

Docker 볼륨 영속성 검증
    Docker 볼륨을 생성하고 컨테이너에 연결한다.
    컨테이너 삭제 전/후로 데이터를 확인하여 데이터가 유지됨을 증명한다.
    기술 문서에 생성/연결/검증 절차(명령+출력)를 포함한다.

Git 설정 및 GitHub 연동
* Git 사용자 정보/기본 브랜치 설정을 완료하고 git config --list 결과를 기록한다.
* GitHub 로그인 및 저장소 연동을 완료하고, 연동 증거(스크린샷 등)를 기술 문서에 첨부한다.
보안 및 개인정보 보호
* 기술 문서/로그/스크린샷에 토큰, 비밀번호, 개인키, 인증 코드 등이 포함되지 않도록 마스킹한다.
* 의심되는 민감정보가 노출된 경우, 즉시 히스토리/문서에서 제거하고 재발급 절차를 수행한다 (가능한 범위에서).

7. 제약 사항

    제출 방식
        제출은 GitHub Repository 링크로 진행한다.
        기술 문서(README.md 등)에 수행 로그와 증거가 모두 포함되어야 한다. (별도 파일로 분리하는 것은 가능하나, README에서 링크로 접근이 가능해야 한다.)
    실행 방식
        모든 작업은 터미널(CLI) 기반으로 수행한다.
        Dockerfile은 직접 작성해야 한다.
        포트 매핑과 마운트/볼륨은 직접 설정하고 동작을 검증해야 한다.
    증거 수집 규칙
        캡처/로그에는 “명령어 입력”과 “출력 결과”가 함께 포함되어야 한다.
        브라우저 접속 증거는 주소창(포트 포함)과 응답 화면이 함께 보이도록 한다.
        민감정보는 로그/이미지에 남기지 않는다(마스킹 필수).
    재현성
        README만 보고도 평가자가 동일 절차를 따라 결과물을 확인할 수 있어야 한다.
        특정 개인 PC에 종속된 경로/설정이 있다면, 대체 방법 또는 주의사항을 함께 기록한다.

####################################################################################################

(1) 프로젝트 개요  
docker desktop 및 터미널의 활용법을 익혀서, 추후 AI 관련 프로젝트를 원활히 진행하기 위한 작업 환경 구축

(2) 실행 환경(OS/쉘/터미널, Docker 버전, Git 버전)  
OS: Windows 11 pro 26H2  
Linux 환경 : WSL2 설치 및 Ubuntu 사용  
Shell: Bash (Ubuntu와 Git Bash에서 사용)  
Terminal: Visual Studio Code Integrated Terminal  

Docker 버전  
```
omg@DESKTOP-6MDPNB3:~$ docker --version
Docker version 29.9.2, build a82e528
omg@DESKTOP-6MDPNB3:~$ docker info
Client:
 Version:    29.9.2
 Context:    default
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.152.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-agent
    ...
```
Git 버전
```
omg@DESKTOP-6MDPNB3 UCRT64 ~$ git --version
git version 2.56.0.windows.4
```

(3) 수행 체크리스트 (터미널/권한/Docker/Dockerfile/포트/마운트/볼륨/Git/GitHub)  
- [x] 터미널 기본 조작 및 폴더 구성  
> docker로 가상 우분트 환경 만들기  
```
omg@DESKTOP-6MDPNB3:~$ docker start mybox
mybox
omg@DESKTOP-6MDPNB3:~$ docker exec -it mybox bash
root@901195528e5d:/# 
```
> pwd : 현재 작업 중인 디렉터리(폴더)의 전체 절대 경로를 화면에 출력
```
root@901195528e5d:/home/ubuntu# pwd
/home/ubuntu
```
> 현재 디렉토리안에 있는 일반파일과 폴더를 보여주는 명령어 ls
```
root@901195528e5d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  temp  tmp  usr  var
```
> ls -la : -la 옵션은 longformat, 숨긴파일도 포함해서 보여줌  
```
root@901195528e5d:/# ls -la
total 64
drwxr-xr-x   1 root root 4096 Nov  4 07:41 .
drwxr-xr-x   1 root root 4096 Nov  4 07:41 ..
-rwxr-xr-x   1 root root    0 Nov  4 07:10 .dockerenv
drwxr-xr-x   2 root root 4096 Oct 25 10:26 .rock
lrwxrwxrwx   1 root root    7 Apr 20  2026 bin -> usr/bin
drwxr-xr-x   2 root root 4096 Apr 20  2026 boot
...
```
> cd 명령어로 폴더 이동  
```
root@901195528e5d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  memo.txt  mnt  opt  proc  root  run  sbin  srv  sys  temp  tmp  usr  var
root@901195528e5d:/# cd home
root@901195528e5d:/home# ls
ubuntu
root@901195528e5d:/home# cd ..
root@901195528e5d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  memo.txt  mnt  opt  proc  root  run  sbin  srv  sys  temp  tmp  usr  var
```
> mkdir로 temp, mydir 폴더 만들고, touch로 file.txt 이름의 빈 파일 만들기  
```
root@901195528e5d:/# mkdir temp
root@901195528e5d:/# cd temp
root@901195528e5d:/temp# touch file.txt
root@901195528e5d:/temp# mkdir mydir
root@901195528e5d:/temp# ls
file.txt  mydir
```
> echo로 단순 문장이 있는 file2.txt 만들고, cat으로 file2.txt 내용 출력   
```
root@901195528e5d:/temp# echo "what the hell" > file2.txt
root@901195528e5d:/temp# ls
file.txt  file2.txt  mydir
root@901195528e5d:/temp# cat file2.txt
what the hell
```
> cp 명령어로 파일을 복사하기. 복사하는 위치도 지정가능
```
root@901195528e5d:/temp# cp file2.txt file3.txt
root@901195528e5d:/temp# ls
file.txt  file2.txt  file3.txt  mydir
root@901195528e5d:/temp# cp file3.txt /temp/mydir/
root@901195528e5d:/temp# cd mydir
root@901195528e5d:/temp/mydir# ls
file3.txt
```
> mv 명령어로 파일을 이동시킴 (file2.txt 를 temp폴더에서 temp의 하위폴더인 mydir로 이동)  
```
root@901195528e5d:/temp# mv file2.txt /temp/mydir
root@901195528e5d:/temp# ls
file.txt  file3.txt  mydir
root@901195528e5d:/temp# cd mydir
root@901195528e5d:/temp/mydir# ls
file2.txt  file3.txt
```
> mv 명령어로 폴더나 파일의 이름 변경도 가능.  
```
root@901195528e5d:/temp/mydir# mv file3.txt file4.txt
root@901195528e5d:/temp/mydir# ls
file2.txt  file4.txt
```

- [x] 권한 변경 실습  
아래는 temp 폴더 안의 파일과 폴더.  
폴더의 권한설정은 drwxr-xr-x 파일은 -rw-r--r--
```
root@901195528e5d:/temp# ls -l
total 8
-rw-r--r-- 1 root root    0 Nov  5 11:37 file.txt
-rw-r--r-- 1 root root   14 Nov  5 12:19 file3.txt
drwxr-xr-x 2 root root 4096 Nov  5 13:02 mydir
```
아래는 mydir 폴더의 권한을 700으로, file.txt 파일의 권한을 600으로 변경  
소유자에게만 read, write이 남아있는 걸을 file3.txt의 권한을 보면 비교 가능  
```
root@901195528e5d:/temp# chmod 700 mydir
root@901195528e5d:/temp# chmod 600 file.txt
root@901195528e5d:/temp# ls -l
total 8
-rw------- 1 root root    0 Nov  5 11:37 file.txt
-rw-r--r-- 1 root root   14 Nov  5 12:19 file3.txt
drwx------ 2 root root 4096 Nov  5 13:02 mydir
```

- [x] Docker 설치/점검  
```
omg@DESKTOP-6MDPNB3:~$ docker --version
Docker version 29.9.2, build a82e528
omg@DESKTOP-6MDPNB3:~$ docker info
Client:
 Version:    29.9.2
 Context:    default
 ...
```

- [x] hello-world 실행 + ubuntu image 활용  
이미 만들어져있는 기본 이미지 중 하나인 hello-world 이미지를 기반으로 컨테이너 생성  
```
omg@DESKTOP-6MDPNB3:~$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
d5e71e642bf5: Download complete 
Digest: sha256:c567b231f65614688711a75f31518a064258a51d442117a3e771a3a8fdba78ef
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```
아래는 ubuntu 이미지를 기반으로 mybox1이라는 이름의 컨테이너 생성  
-it 옵션은 대화형 입력을 유지(Interactive)하면서 터미널을 할당  
"root@c4bf5d600a47:/#"는 새로운 리눅스 환경에 접속된 것을 보여줌  
```
omg@DESKTOP-6MDPNB3:~$ docker run -it --name mybox1 ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
f3f25b84a7d7: Pull complete 
edafa32f5d9e: Pull complete 
21594acc2d8b: Download complete 
Digest: sha256:6a06111c3b0a8600feadb270aec771dc239bf407b25625f93f44ab00e11f4d3c
Status: Downloaded newer image for ubuntu:latest
root@c4bf5d600a47:/#
root@c4bf5d600a47:/# exit
exit
```
아래는 현재 생성된 컨테이너의 목록을 보여줌.  
hello-world 이미지로 컨테이너 만들때 이름을 특정하지 않아서 'adoring_fermi'라는 
랜덤 이름이 붙게됨. ubuntu 이미지로 만들어진 컨테이너인 mybox1도 확인 가능.  
두 컨테이너는 사용 중이지 않아서 status가 Exited 로 표시됨.  
```
omg@DESKTOP-6MDPNB3:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED          STATUS                      PORTS     NAMES
c4bf5d600a47   ubuntu        "/bin/bash"   13 minutes ago   Exited (0) 33 seconds ago             mybox1
b9e3fa3d3bef   hello-world   "/hello"      49 minutes ago   Exited (0) 49 minutes ago             adoring_fermi
```
아래 코드는 현재 생성된 이미지를 보여줌.
```
omg@DESKTOP-6MDPNB3:~$ docker images
                                                                                                                                         i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       25.9kB         9.49kB    U   
ubuntu:latest        6a06111c3b0a        163MB         45.9MB    U   
omg@DESKTOP-6MDPNB3:~$ 
```
아래 코드는 mybox1 컨테이너의 로그를 보여준다.
```
omg@DESKTOP-6MDPNB3:~$ docker logs mybox1
root@c4bf5d600a47:/# exit
exit
```
아래는 컨테이너가 사용하는 리소스 현황을 보여주는 명령어인데 모든 컨테이너가 exited상태라 아무것도 나오지 않음.
```
omg@DESKTOP-6MDPNB3:~$ docker stats --no-stream
omg@DESKTOP-6MDPNB3:~$ 
```
docker start [컨테이너 이름] : 이미 생성된 컨테이너 실행.  
mybox1이 running 상태로 바뀌기에 리소스 상태가 출력된다.
```
omg@DESKTOP-6MDPNB3:~$ docker start mybox1
mybox1
omg@DESKTOP-6MDPNB3:~$ docker stats --no-stream
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O     PIDS
c4bf5d600a47   mybox1    0.00%     3.672MiB / 7.576GiB   0.05%     1.17kB / 126B   16.8MB / 0B   1
```
또한, 컨테이너 현황을 보면 mybox1 컨테이너의 status가 Up으로 활성화되어 있음을 알 수 있다.
```
omg@DESKTOP-6MDPNB3:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED          STATUS                      PORTS     NAMES
c4bf5d600a47   ubuntu        "/bin/bash"   23 minutes ago   Up 2 minutes                          mybox1
b9e3fa3d3bef   hello-world   "/hello"      59 minutes ago   Exited (0) 59 minutes ago             adoring_fermi
omg@DESKTOP-6MDPNB3:~$ 
```
전처럼, mybox1의 우분투 터미널로 접속하려면 exec 명령어를 이용.  
그러기 위해선 컨테이너가 start 명령어로 이미 실행 중이 상태여야 한다.  
그리고 docker stop [컨테이너 이름]을 사용해야만 그 컨테이너 status가 exited로 바뀐다.
```
omg@DESKTOP-6MDPNB3:~$ docker exec -it mybox1 bash
root@ea5249983202:/# ls  
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ea5249983202:/# echo 'you are fired'
you are fired
root@ea5249983202:/#
```

- [x] Dockerfile 빌드/실행  
/mnt/c/Users/omg/vsc_practice 이 폴더 안에 dockerfile을 미리 만들어 두고 새로운 이미지 생성.  
dockerfile 내부에는 웹서버 이미지(nginx)를 활용하도록 기술되어 있음.  
추가로 그 폴더 안에 커스텀 index.html 파일을 만들어서, 추후 nginx로 컨테이너 만들 때 커스텀 html파일이 출력되도록 설계. 
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker build -t myimg .
[+] Building 3.0s (7/7) FINISHED                                                        docker:default
 => [internal] load build definition from Dockerfile                                    0.0s
 => => transferring dockerfile: 111B                                                    0.0s
 ...
```
아래와 같이 myimg:latest 이미지가 새로 생성됨을 확인 가능
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker images
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       25.9kB         9.49kB    U   
myimg:latest         de7adb356b85        241MB         63.9MB        
ubuntu:latest        6a06111c3b0a        163MB         45.9MB    U   
```
아래는 커스텀 이미지(myimg)로 myweb1이라는 컨테이너 생성 코드.  
-d = 컨테이너가 백그라운드에서 실행.  
-p 8080:80 = 포트 매핑. 내컴퓨터의 8080포트와 웹서버의 80포트를 연결.
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker run -d -p 8080:80 --name myweb1 myimg
16c1f142cdea009fcc10e63cba7b4da4a6e96a5f4858c11f2006fed22cce888a
```
curl 명령어를 이용하여 웹서버가 아래와 같은 페이지를 출력하는 것을 확인 가능  
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ curl http://localhost:8080
<!-- index.html -->
<!DOCTYPE html>
<html>
<head><meta charset="utf-8"><title>My First Container</title></head>
<body>
    <h1>안녕하세요, 홍길동입니다</h1>
    <p>이 페이지는 Docker 컨테이너 안에서 실행되고 있습니다.</p>
</body>
</html>
```
브라우저에 http://localhost:8080 에 접속하면 아래와 같은 웹페이지가 로드됨.  
<img width="987" height="399" alt="image" src="https://github.com/user-attachments/assets/1671f7cf-7ede-4b04-a621-6eee795165be" />  

- [x] 포트 매핑 접속(2회)

myimg 이미지로 웹서버와 연결되는 컨테이너를 생성하는데 내 컴퓨터의 포트 넘버만 8081로 변경한다.  
그럼에도 브라우저에서 localhost:8081 로 만들어진 웹페이지가 로드됨.  
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker run -d -p 8081:80 --name myweb2 myimg
8b699e6adf38e501dcaddcddf7a7100e6f77d93344fe13296bcadf14f42b6f1d
```
이번에는 웹서버 포트 넘버를 80에서 81로 변경해본다.  
이 경우에는 브라우저에서 웹페이지 로드 실패.   
즉, 웹서버 포트는 80으로 유지되어야 하며 이는 nginx 이미지에 미리 설정된 웹서버 포트넘버 같음.
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker run -d -p 8082:81 --name myweb3 myimg
a1ccbed2da2369ee273dc05cbc64ed2b3437df94fba07ff9d37143a3159f3774
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ curl http://localhost:8082
curl: (56) Recv failure: Connection reset by peer
```

- [x] 바인드 마운트 반영
nginx 이미지로 웹서버 연결되는 컨테이너를 만드는 코드. host포트는 8084. 컨테이너 이름은 myweb-live1.  
-v ${PWD}:/usr/share/nginx/html => 현재 디렉토리가 컨테이너 내부에 존재하는 /usr/share/nginx/html 폴더와 연결됨.  
html폴더 안의 파일들은 사라지고 디렉토리 내부 파일들로 교체된다.
```
docker run -d -p 8084:80 --name myweb-live1 -v ${PWD}:/usr/share/nginx/html nginx
```
로컬포트 8084에서 웹페이지 정상 로드.  
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ curl http://localhost:8084
<!-- index.html -->
<!DOCTYPE html>
<html>
<head><meta charset="utf-8"><title>My First Container</title></head>
<body>
    <h1>안녕하세요, 홍길동입니다</h1>
    <p>이 페이지는 Docker 컨테이너 안에서 실행되고 있습니다.</p>
</body>
</html>
```
index.html을 수정하고 저장하는 것만으로 myweb-live1 컨테이너의 html폴더 안의 내용이 변경됨.  
호스트와 컨테이너가 포트로 (8084:80) 연결되어 있기에 수정 내용이 바로 적용됨. (홍길도 -> 홍길똥)
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ curl http://localhost:8084
<!-- index.html -->
<!DOCTYPE html>
<html>
<head><meta charset="utf-8"><title>My First Container</title></head>
<body>
    <h1>안녕하세요, 홍길똥입니다</h1>
    <p>이 페이지는 Docker 컨테이너 안에서 실행되고 있습니다.</p>
</body>
</html>
```


- [x] 볼륨 영속성
먼저 mydata라는 볼륨을 생성. 존재하는 볼륨 리스트 업.
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker volume create mydata
mydata
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker volume ls
DRIVER    VOLUME NAME
local     mydata
```
ubuntu 가상 환경의 새로운 컨테이너를 생성하는데, 방금 전 만든 볼륨인 mydata와 우분투 내에 data1이라는 폴더를 만들고 싱크한다.
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker run -it --name vol-con1 -v mydata:/data1 ubuntu
root@03643eecd1ed:/# ls                                    
bin  boot  data1  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@03643eecd1ed:/# cd data1
root@03643eecd1ed:/data1# ls
```
컨테이너 내부에 /data1 폴더에 단순한 문장이 적힌 memo.txt 파일 생성.  
```
root@03643eecd1ed:/data1# echo 'hello volume' > /data1/memo.txt
root@03643eecd1ed:/data1# ls
memo.txt
```
이 memo.txt 파일은 docker desktop에서 mydata라는 볼륨에도 저장된 것을 확인 할 수 있다.  
<img width="1283" height="714" alt="image" src="https://github.com/user-attachments/assets/3829cb2d-0d09-45af-b167-044cf39ef49b" />  

이제 이 컨테이너를 삭제 후에도 볼륨안에 memo.txt 파일이 남아있는지 확인해본다.  
기존 컨테이너 삭제 후 새로운 컨테이너를 만드는데, mydata 볼륨을 새 컨테이너의 내부에 /data 폴더와 연동시킨다.  
그리고 cat /data/memo.txt 명령어로 memo.txt 파일내용을 출력한다. 먄약 볼륨 내부에 memo.txt 파일이 없어졌거나  
새 컨테이너 내부의 /data 폴더와 연결이 안되어 있다면 memo.txt 내용은 출력되지 못하였을 것이다.  
```
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker rm vol-con1
vol-con1
omg@DESKTOP-6MDPNB3:/mnt/c/Users/omg/vsc_practice$ docker run -it --rm -v mydata:/data ubuntu cat /data/memo.txt
hello volume
```

- [x] Git 설정 + VSCode GitHub 연동




