---
layout: post
title: Docker에 Jenkins 설치하기(WSL2)
comments: true
categories: study
---

이 포스팅은 Windows 10에 WSL2 과 docker desktop 이 설치되어있음을 기준으로 설명되었습니다.

# Jenkins 설치

작업 폴더로 이동하여 jenkins 도커 이미지를 위한 `Dockerfile` 을 생성합니다.

* Dockerfile

```
FROM jenkins/jenkins:jdk11

#도커를 실행하기 위한 root 계정으로 전환
USER root

#도커 설치
COPY docker_install.sh /docker_install.sh
RUN chmod +x /docker_install.sh
RUN /docker_install.sh

#설치 후 도커그룹의 jenkins 계정 생성 후 해당 계정으로 변경
RUN groupadd -f docker
RUN usermod -aG docker jenkins
USER jenkins
```

`Dockerfile` 에서 docker 를 설치하기 위한 쉘 스크립트가 들어있는데 이 스크립트도 같은 디렉터리에 생성해줍니다.

* docker_install.sh

```
#!/bin/sh
apt-get update && \
apt-get -y install apt-transport-https \
  ca-certificates \
  curl \
  gnupg2 \
  zip \
  unzip \
  software-properties-common && \
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
$(lsb_release -cs) \
stable" && \
apt-get update && \
apt-get -y install docker-ce
```

이후, jenkins docker 이미지를 빌드해줍니다.

```
$ docker build -t jenkins/jenkins .
```

이미지 빌드가 완료되면 이제 jenkins 도커 컨테이너를 띄워봅시다.

```
$ docker run -d -p 8080:8080 --name=Jenkins-master -v <작업 디렉터리>/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/do
cker.sock jenkins/jenkins
```
이때, 각 옵션을 확인해보면
* `-d`: 백그라운드 실행
* `--name`: 컨테이너의 이름을 설정해줍니다
* `p`: 컨테이너의 포트를 인스턴스(혹은 로컬)의 포트와 매핑합니다. `:` 를 기준으로 왼쪽은 로컬포트, 오른쪽은 도커 이미지의 포트를 뜻합니다.
* `-v`: 컨테이너의 경로를 마운트해줍니다. 이는, 컨테이너가 종료되었다 다시 부팅되어도 설정 파일을 그대로 남겨놓기 위함입니다.

```
$ docker ps
```

도커 컨테이너가 정상적으로 띄워졌는지 확인해줍니다.

---

# Jenkins 설정

`localhost:8080` 로 접속해봅시다.
![Getting started](/images/jenkins/jenkins-started.png)

password 를 찾기 위해서는 두 가지 방법이 있습니다.
## 1. Docker Desktop 에서 jenkins 컨테이너를 클릭하여 로그를 확인하기
  그러면 아래와 같이 로그로 password 를 확인할 수 있습니다.
  ![password](/images/jenkins/jenkins-password.png)
## 2. docker 명령어를 통해 확인하기

 ```
 $ docker exec <Contaier ID> cat /var/jenkins_home/secrets/initialAdminPassword
 ```

참고로, 컨테이너 ID 는 `docker ps` 를 통해 알아낼 수 있습니다.

비밀번호 입력이 끝나면 플러그인을 설치합니다.
![jenkins plugin](/images/jenkins/jenkins-plugin.png)

특별히 필요한 플러그인이 존재하는 것이 아니라면 왼쪽 탭을 눌러 설치해줍니다.
![jenkins plugin download](/images/jenkins/jenkins-plugin-download.png)


그 후, 계정 정보를 생성합니다.
![id](/images/jenkins/jenkins-id.png)

특별히 사용하는 public domain 이 있는 것이 아니라면 localhost 를 사용하여 설정하여 마무리해줍니다.
![url](/images/jenkins/jenkins-url.png)

# jenkins 테스트

jenkins 로 각 서비스를 빌드하고 docker 컨테이너에 배포를 해야하므로 docker 를 사용하여야 합니다.
그래서 사용이 가능한지 간단한 스크립트를 통해 테스트해봅시다. 

먼저, 새로운 아이템을 생성해줍니다.
![items](/images/jenkins/jenkins-item.png)

간단한 테스트를 위해 만드는 job 이므로 아이템의 이름은 `docker test` 로 지어주고 `Freestyle project` 로 만들어줍니다.
![items](/images/jenkins/jenkins-item-1.png)

![items](/images/jenkins/jenkins-item-2.png)
![items](/images/jenkins/jenkins-item-3.png)
빌드 유발에서 `Build > Execute shell` 을 선택합니다. 선택하게 되면 아래 shell command 를 입력하는 창이 나오는데, 간단하게 docker 정보를 확인해봅시다.

```
docker ps
```

![items](/images/jenkins/jenkins-item-4.png)

저장을 하게되면 프로젝트 창으로 넘어가는데 여기에서 지금 빌드(Build Now)를 클릭하면 빌드가 진행되고 Build History 에 기록이 올라옵니다.

![jobs](/images/jenkins/jenkins-job-1.png)

해당 빌드 기록의 console output 을 확인하면 
![jobs](/images/jenkins/jenkins-job-2.png)

정상적으로 입력한 커맨드가 잘 작동하는 것을 볼 수 있습니다.

이제, jenkins 설정이 끝났습니다. 다음으로 해야할 것은 github action 과 webhook 을 통한 자동 배포 설정을 해보도록 하겠습니다.

궁금한 점이 있다면 많은 질문 부탁드립니다.
감사합니다!

# 참고자료
[https://velog.io/@hind_sight/Docker-Jenkins-도커와-젠킨스를-활용한-Spring-Boot-CICD](https://velog.io/@hind_sight/Docker-Jenkins-도커와-젠킨스를-활용한-Spring-Boot-CICD)
[https://www.hanumoka.net/2019/10/14/docker-20191014-docker-jenkins-docker-in-docker/](https://www.hanumoka.net/2019/10/14/docker-20191014-docker-jenkins-docker-in-docker/)