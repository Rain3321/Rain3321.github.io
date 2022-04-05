---
layout: post
title: Gradle dependencies의 기본 구조
comments: true
categories: study
---

java gradle dependencies 블록에서 `api`, `implementation`, `complieOnly`, `runtimeOnly`, `annotaionProcessor` 을 사용하는데 어떤 동작을 하는지 명확하게 알지 못해 이참에 정리해보자.

참고 문서: [gradle document](https://docs.gradle.org/current/userguide/dependency_management_for_java_projects.html#sec:configurations_java_tutorial)

---

# api

`The dependencies required to compile the production source of the project which *are* part of the API exposed by the project. For example the project uses Guava and exposes public interfaces with Guava classes in their method signatures.`

프로젝트에 의해 노출된 API의 일부인 프로젝트의 프로덕션 코드를 컴파일시 필요한 의존성. 예를 들어, 이프로젝트가 Guava를 사용하고 그들의 메서드 서명에 guava 클래스의 퍼블릭 인터페이스가 노출되어 있다.

의존 라이브러리 수정 시 해당 모듈을 의존하고 있는 모듈 역시 재빌드가 이루어짐.

- A(api) ← B ← C 일 때, C 에서 A까지 접근 가능
- A 수정 시 C 까지 재빌드

# implementation

`The dependencies required to compile the production source of the project which *are not* part of the API exposed by the project. For example the project uses Hibernate for its internal persistence layer implementation.`

프로젝트에 의해 API의 일부로 노출되지 않은 프로젝트의 프로덕션 코드를 컴파일 시 필요한 의존성. 예를 들어, 내부적인 영속성 레이어 구현을 위한 하이버네이트를 사용하는 프로젝트

의존 라이브러리 수정시 본 모듈까지만 재빌드

- A(api) ← B ← C 일 때, C 에서 A를 접근할 수 없음
- A 수정시 B 까지만 재빌드

# compileOnly

[https://blog.gradle.org/introducing-compile-only-dependencies](https://blog.gradle.org/introducing-compile-only-dependencies)

컴파일 시에만 필요하고 런타임 시에는 필요하지 않는 의존성에 대해 사용함

빌드 결과물에 포함하지 않음.

# runtimeOnly

runtime 시에만 필요한 라이브러리의 경우 사용

# annotationProcessor

[https://blog.gradle.org/incremental-compiler-avoidance#about-annotation-processors](https://blog.gradle.org/incremental-compiler-avoidance#about-annotation-processors)

일반적으로 의존성 주입이나 보일러플래이트 코드를 줄이기 위한 애노테이션으로 많이 사용한다. 

하지만 애노테이션을 잘못 사용하면 빌드 시에 블랙박스 지점이 될 수 있기 때문에 그래들 상에서 애노테이션 프로세싱이 필요없다고 예측되는 경우 빌드에서 제외시킴[https://developer.android.com/studio/build/dependencies?utm_source=android-studio#dependency_configurations](https://developer.android.com/studio/build/dependencies?utm_source=android-studio#dependency_configurations)

`주석 프로세서인 라이브러리에 종속성을 추가하려면 반드시 annotationProcessor구성을 사용하여 주석 프로세서 클래스 경로에 추가해야 합니다. 그 이유는 이 구성을 사용하면 컴파일 클래스 경로를 주석 프로세서 클래스 경로와 분리하여 빌드 성능을 향상할 수 있기 때문입니다.`