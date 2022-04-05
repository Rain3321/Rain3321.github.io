---
layout: post
title: MyBatis-Invalid bound statement (not found) 에러
comments: true 
categories: study
---

![에러](https://user-images.githubusercontent.com/39397110/116561235-1a0b9d00-a93d-11eb-9ece-ed4dfd661906.png)

위와 같은 에러가 나타나는 이유는 크게 2가지가 있다. 

1. Mapper Interface와 xml에 오타가 있는 경우
  
2. classpath에 경로가 잘못되어 있는 경우

---

1과 같은 경우는 mapper를 오타로 인해 잘못 설정하는 경우도 있고

mapper Service의 method 이름과 xml의 id 이름이 다를 때 자주 발생한다.

꼭 놓치지 말고 확인하자.
```
// Service 메소드
public List<Product> selectAll() throws Exception {
}

// mapper id
<mapper namespace="com.ssafy.model.repository.ProductRepo">
	
	<select id="selectAllProduct" resultType="product">
		
	</select>
</mapper>

```
---

2번의 경우 디렉터리를 직접 타고 들어가면서 하나하나 확인하는 것이 좋다.
resources 밑에 경로를 만들 경우 package처럼 ```com.spring.repository..``` 처럼 만들어지는 것이 아니라
```com/spring/repository/...``` 처럼 만들어지기 때문에 접근이 불가능한 경우가 발생할 수 있다.