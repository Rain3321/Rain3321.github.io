---
layout: post
title: Spring Cloud 란 무엇인가? (MSA)
comments: true
categories: study
---

Spring Cloud 는 MSA 를 지향하는 아키텍처를 쉽게 구성할 수 있도록 지원하는 Spring Boot 기반의 프레임워크입니다.

Spring boot 기반의 프로젝트에서 config 관리나 service discovery 와 같은 공통적인 패턴을 스프링 라이브러리로 제공하여 개발자로 하여금 이런 부분에 대한 개발 부담을 덜고 서비스 기능을 개발하는 것에 더 집중할 수 있습니다.

Spring 프로젝트에서 제공하는 Spring Cloud 기능 목록은 다음과 같습니다.

* Distributed/versioned configuration (분산 버전 설정)
* Service registration and discovery (서비스 등록 및 디스커버리)
* Routing (라우팅)
* Service-to-service calls (서비스간 호출)
* Load balancing (로드밸런싱, 서비스 부하 분산)
* Circuit Breakers (서킷 브레이커)
* Global locks (글로벌 락)
* Leadership election and cluster state (리더 선출 및 클러스터 상태 관리)
* Distributed messaging (분산 메시징)

현재 Spring Cloud 의 전체 기능은 [Spring.io](https://spring.io/projects/spring-cloud) 에서 확인할 수 있습니다.

![spring.io](/images/spring-cloud/spring-cloud-io.png)


이제, spring cloud의 기능을 하나씩 사용해보며 개발한 어플리케이션을 docker 컨테이너를 통해 동작할 수 있도록 구성해보겠습니다.
