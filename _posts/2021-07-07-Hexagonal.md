---
layout: post
title: 헥사고날 아키텍처(Hexagonal Architecture)
comments: true
categories: web
---

```
Java Spring을 공부하면서 기록하는 포스트입니다.
내용이 잘못되었거나 문제가 있다면 댓글이나 이메일로 알려주시면 감사하겠습니다.
```

## 서론

---

MSA로 애플리케이션을 구성하게 되면 다른 여러 애플리케이션을 호출하는 시스템도 존재할 수 있고 저장을 위한 저장소가 존재할 수 있기 때문에 다양한 인터페이스가 구현되어야 한다.
이런 상황에서 등장한 헥사고날 아키텍처는 '포트&어댑터 아키텍처'라고도 불리며 비즈니스 로직을 담당하는 **내부 영역**과 인터페이스 처리를 담당하는 **외부 영역**으로 나뉜다.

## 본론

---

![헥사고날 아키텍처 설명 이미지](https://res.cloudinary.com/practicaldev/image/fetch/s--43uphorj--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/33ru7jmqzice8bfsq8of.png)
`출처 : https://dev.to/peholmst/domain-driven-design-and-the-hexagonal-architecture-2o87`

### 헥사고날 아키텍처 구성

---

- #### 내부 영역

  순수한 도메인과 이를 활용할 서비스 로직이 구현되어 있는 영역이며 외부 영역에 연계될 수 있는 포트를 갖고 있다.

- #### 외부 영역

  외부에서 들어오는 요청을 처리하는 인바운드 어댑터(Inbound Adapter)와 비즈니스 로직에 의해 호출되어 외부와 연계되는 아웃 바운드 어댑터(Outbound Adapter)로 구성되어 있다.

- #### 포트(Port)

  - 인 바운드(Inbound) 포트
    내부 영역 사용을 위해 사용되는 API로 외부 영역의 인 바운드 어댑터가 호출한다.

  - 아웃 바운드(Outbound) 포트
    내부 영역이 외부를 호출하는 방법을 정의한다.

### 헥사고날 아키텍처 특징

---

- 내부 영역 -> 외부 영역으로는 접근 가능하지만 반대로는 불가능
- 비즈니스 로직을 표현하는 내부 영역은 어댑터에 의존하는 것이 아니라 내부 영역이 제공하는 포트에 의존하여 구현함
- DB가 바뀌는 등의 기술적인 세부사항이 변경되어도 도메인 코드의 수정없이 외부(인프라) 영역 코드를 추가하고 그에 따른 어댑터와 포트만 구현해주면 대응이 쉽게 가능함.

---

시나리오를 통한 코드를 작성해보는 것은 2편에 이어서 작성하겠습니다.
