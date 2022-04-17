---
layout: post
title: Spring Cloud Config private repository 에서 사용하기
comments: true
categories: study
---

Spring Config Server 를 띄울 때 github private repository 로 사용할 경우 겪은 시행착오를 바탕으로 작성한 글입니다.

## 직접 id/pw 입력하기

말 그대로 yaml 파일에 github의 id 와 password 를 직접 입력하는 것입니다. 하지만 이 방법은 id 와 비밀번호가 고스란히 노출되기 때문에 추천하지 않습니다.

``` yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: <http git repo url>
          username: <github id>
          password: <github password>
```

## 공개키를 이용하기

먼저 SSH key 를 생성합니다.

```
$ ssh-keygen -m PEM -t rsa -b 4096 -C "github 이메일"
```

경로는 일반적으로 `~/.ssh` 밑에 생성됩니다. 해당 경로로 들어가보면 공개키와 개인키, 2개의 파일이 생성됩니다.

### github 에 공개키 설정하기

공개키는 github 에서 사용하고 개인키는 config server 에서 사용해줍니다.

![github ssh](/images/spring-cloud/config/git-ssh2.png)

Settings > SSH and GPG keys 탭에서 추가할 수 있습니다.

### 추가

```
ERROR: You're using an RSA key with SHA-1, which is no longer allowed. Please use a newer client or a different key type. Please see https://github.blog/2021-09-01-improving-git-protocol-security-github/ for more information.
```

rsa 로 키를 생성할 경우 해당 에러 로그가 뜹니다.
22.03.15 이후로 github 에서 RSA 를 이용한 SHA-1 인증을 더이상 지원하지 않는다고 합니다. ECDSA 또는 Ed25519 를 사용하라고 권고하고 있습니다.
그래서 키를 다시 만듭시다. Ed25519 의 경우 spring cloud config 가 지원하지 않습니다.

```
$ ssh-keygen -m PEM -t ecdsa -b 4096 -C "github 이메일"
```

그외에 github 에 공개키를 등록하는 것은 동일합니다.

### config server에 개인키 추가하기

config server 어플리케이션의 application.yml 에 설정을 수정합니다.

``` yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: <github ssh url>
          ignore-local-ssh-settings: true
          private-key: |
            -----BEGIN EC PRIVATE KEY-----
            your-key-here
            your-key-here
            your-key-here
            -----END EC PRIVATE KEY-----
          host-key:  AAAAE2VjZHNhLXNoYTItbm.....
          host-key-algorithm: ecdsa-sha2-nistp256
```

설정 하나하나씩 확인해보면 

* uri: github 레포에서 SSH 주소를 가져올 수 있습니다. 이걸 복사해옵시다.

![github ssh](/images/spring-cloud/config/git-ssh3.png)

* ignore-local-ssh-settings: 파일 시스템 베이스의 ssh 설정 대신 속성 기반의 ssh 설정을 사용하려면 `true` 로 설정합니다. 우리는 git 을 통해 사용하기 때문에 꼭 true 로 설정해주세요.
* private-key: 위에서 생성한 개인키를 입력해줍니다. 이때, 백슬래쉬(`|`) 빼놓지 마시고 `-----BEGIN EC PRIVATE KEY-----` 와 `-----END EC PRIVATE KEY-----` 도 그대로 적어주세요. 
* host-key, host-key-algorithm: 기본적으로 rsa 를 사용한다고 설정되어있기 때문에 명시적으로 ecdsa 를 사용한다고 설정해주어야 합니다. 이를 확인하는 방법

```
$ ssh-keyscan -t ecdsa github.com
```

![github ssh](/images/spring-cloud/config/git-ssh4.png)

중간에 있는 `ecdsa...` 를  **host-key-algorithm** 에, 그 뒤에 따라오는 키값을 **host-key** 로 입력해주면 됩니다.

끝!
