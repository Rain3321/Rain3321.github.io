---
layout: post
title: Lombok 애노테이션 공부(1)
comments: true
categories: web
---

```
Java Spring을 공부하면서 기록하는 포스트입니다.
내용이 잘못되었거나 문제가 있다면 댓글이나 이메일로 알려주시면 감사하겠습니다.
```

## Lombok 라이브러리

---

자바 개발시 많이 사용하는 getter/setter/toString/hashCode/생성자와 같은 메소드들을 직접 만들다보면 코드의 길이도 굉장히 길어지고 지저분해지기 쉽다.
이를 해결하기 위해 사용하는 라이브러리가 롬복(Lombok)이다.
많이 사용하는 애노테이션을 알아보자.

## 1. @Getter / @Setter

---

롬복에서 가장 많이 사용되는 애노테이션으로 클래스에 선언된 필드를 getXxx(), setXxx() 메소드를 자동으로 생성해준다.

### 롬복 라이브러리

```java
@Getter
@Setter
public class Receipt {

  private String orderNo;
  private int price;
}
```

### 바닐라 자바

```java
public class Receipt {

  private String orderNo;
  private int price;

  public String getOrderNo(){
    return this.orderNo;
  }
  public String getPrice(){
    return this.price;
  }
  public void setOrderNo(String orderNo) {
    this.orderNo = orderNo;
  }
  public void setPrice(int price) {
    this.price = price;
  }
}
```

한눈에 알아볼 수 있듯 코드의 길이가 간단해지고 복잡함도 줄어드는 것을 볼 수 있다.
또한, 접근 수준을 설정할 수 있는데, 기본적으로 설정을 하지 않으면 public으로 만들어지며
AccessLevel_XXXX에서 `PUBLIC`, `PROTECTED`, `PACKAGE`, `PRIVATE` 4가지 인자를 쓸 수 있다.

```java
@Setter(AccessLevel_PROTECTED)
private String name;
```

```java
private String name;
protected void setName(String name) {
  this.name = name;
}
```

## 2. @ToString

---

getter, setter와 마찬가지로 Lombok을 사용하면 `toString()` 메소드를 쉽게 구현할 수 있다. 기본적으로 구현되어있는 non-static 필드가 정의된 순서대로 출력하며 `exclude`속성을 사용하면 특정 필드를 `toString()` 결과에서 제외한다. 그리고 `callSuper` 속성을 활용하면 부모 객체의 `toString()` 메소드를 같이 호출하거나 제외할 수 있다.(기본 설정은 false)

### 롬복 라이브러리

```java
@ToString(exclude = "password")
public class User {
  private String username;
  private String password;
}
```

### 바닐라 자바

```java
public class User {
  private String username;
  private String password;

  @Override
  public String toString(){
    return "User[username=" + this.username + "]";
  }
}
```

## 3. @EqualsAndHashcode

---

`equals(Object other)`과 `hashCode()` 메소드를 생성해주는 애노테이션으로 `@ToString` 애노테이션과 마찬가지로 `exclude` 속성을 활용해서 특정 필드를 제외시킬 수 있고 `callSuper` 속성을 활용해서 부모 클래스의 필드까지 포함해서 비교할 수 있다.
하지만, 상속을 하나도 받지 않은 상태에서 `callSuper` 속성을 true로 설정하면 컴파일 오류가 날 수 있으니 주의해서 사용해야 한다.

### 롬복 라이브러리

```java
@EqualAndHashCode
public class Product {
  @EqualAndHashCode.Exclude
  private long id;
  private String name;
  private int price;

  public String getName() {
    return this.name;
  }
}
```

### 바닐라 자바

```java
public class Product {
  private long id;
  private String name;
  private int price;

  public String getName() {
    return this.name;
  }

  @Override
  public boolean equals(Object O) {
    if(o == this) return true;
    if(!(o instanceof Product)) return false;
    Product other = (Product) o;
    if(!other.canEqual((Object)this))  return false;
    if(this.getName() == null ? other.getName() != null : !this.getName.equals(other.getName())) return false;
    if(Int.compare(this.price, other.price) != 0) return false;
    return true;
  }

  @Override
  public int hashCode() {
    final int PRIME = 59;
    int result = 1;
    result = (result * PRIME) + (this.name == null ? 43 : this.name.hashCode());
    result = (result * PRIME) + price;
    return result;
  }

  protected boolean canEqual(Object other) {
    return other instanceof Product;
  }
}
```

## 참고 사이트

[project Lombok](https://projectlombok.org/features/all)
