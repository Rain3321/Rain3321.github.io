---
layout: post
title: CrudRepository와 JpaRepository 차이 (Spring)
comments: true
categories: web
---

JpaRepository 는 `PagingAndSortingRepository` 를 상속받고  `PagingAndSortingRepository` 는 `CrudRepository`를 상속받는 구조이다.

`Repository` > `CrudRepository` > `PagingAndSortingRepository` > `JpaRepository`

- `[CrudRepository](http://static.springsource.org/spring-data/data-commons/docs/current/api/org/springframework/data/repository/CrudRepository.html)` : CRUD의 기능을 제공함.
- `[PagingAndSortingRepository](http://static.springsource.org/spring-data/data-commons/docs/current/api/org/springframework/data/repository/PagingAndSortingRepository.html)` : 페이지네이션과 레코드 정렬  메서드를 제공함.
- `[JpaRepository](http://static.springsource.org/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/repository/JpaRepository.html)` : 한번에 레코드를 삭제하거나 영속성 컨텍스트를 플러시 하는등의 jpa와 관련된 메서드를 제공함.

그래서 `JpaRepository` 나  `PagingAndSortingRepository` 의 기능을 사용하지 않을 것이라면 `CrudRepository`을 사용하면 된다.

`JpaRepository` 와 `CrudRepository` 의 차이점을 살펴보자면

`CrudRepository` 의 경우 saveAll() 메서드의 return 객체가 iterable인 반면

![image1](https://user-images.githubusercontent.com/39397110/156362544-eaf5f859-d168-467e-b571-f69ba0292ea5.png)

`JpaRepository` 의 경우 saveAll() 메서드의 return 객체가 List 이다.

![image2](https://user-images.githubusercontent.com/39397110/156362594-d1fba90e-2737-493b-9cd6-c921d6c0391a.png)


번외로, 최근 공부중인 `ElasticsearchRepository` 의 경우 `PagingAndSortingRepository` 을 상속받아 만들어졌다.