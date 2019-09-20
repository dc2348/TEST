---
layout: post
title: '[Java] 올바른 equals() 함수 사용법'
author: hyesoo.shin
date: 2018-05-10 16:30
tags: [Java, JSP]
image: /files/covers/first_post.jpg
comments : true
---

# 올바른 equals() 함수 사용법

equals()는 보통 이런 형태로 많이들 사용할 것입니다.
```Java
변수.equals(비교문자열)
```
이 형태는 변수의 값이 절대적으로 null이 나오지 않을 경우에는 상관이 없습니다.
하지만 requst.getParameter()를 사용해서 변수의 값을 초기화 한다거나 변수의 값이 수시로 바뀔 수 있는 상황에서는 null 이 들어올수 있습니다.
이 형태에서 변수에 null 이 들어오게 되면 Exception 이 발생하나는건 잘 아실겁니다.



하지만
```Java
비교문자열.equals(변수)
```
형태로 문자열을 비교한다면 변수에 null 이 들어와도 Exception 이 발생하지 않습니다.(false 출력됨)



이유는 문자열을 비교할때 주체가 되는 대상이 달라지기 때문입니다.
```Java
변수.equals(비교문자열) : 변수가 주체가 되어서 문자열 비교
비교문자열.equals(변수) : 비교문자열이 주체가 되어서 문자열 비교
```

출처: http://truepia.tistory.com/268 [진실세상을 꿈꾸며]
