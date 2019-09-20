---
layout: post
title: '[javascript] Console 창에 테이블형식으로 데이터 보여주기 - console.table()'
author: hyesoo.shin
date: 2017-11-10 18:20
tags: [javascript]
image: /files/covers/first_post.jpg
comments : true
---

#  Console 창에 테이블형식으로 데이터 보여주기 - console.table()

웹 개발을 하면서 디버깅을 가장 편하고 빠르게 하기위해 `console.log()`를 주로 사용하고 있다. 매일 똑같은 함수만 사용하다 Console Object에 대해 궁금해서 찾아보던 중 `console.table()` 함수가 있다는 것을 알게되었다.

개발을 하면서 가장 많이 찍어보는 것이 API에서 내려온 데이터인데 주로 `console.log`를 통해 아래와 같이 확인을 하고 있었다.
```javascript
var family = {};

family.mother = new Person("Jane", "Smith");
family.father = new Person("John", "Smith");
family.daughter = new Person("Emily", "Smith");
```
위와 같은 객체가 내려온다고 가정할 때,

### console.log()

소스코드
```javascript
console.log(family);
```
결과

![console.table결과](/assets/images/post/2017-11-10-javascript-console/20171110_jc_1.jpg)


그러나, `console.table()` 함수를 사용하면 아래와 같은 결과를 얻을 수 있다.

### console.table()
소스코드
```javascript
console.table(family);
```
결과

![console.table결과](/assets/images/post/2017-11-10-javascript-console/20171110_jc_2.jpg)


`console.table()` 함수를 사용하는 것이 객체를 확인하는데 훨씬 편리할 것 같아 앞으로 이 함수를 많이 이용하려고한다. 그러나 한가지 문제점이라면 object 형식의 데이터가 2depth 이상부터는 console창에서 클릭하여 펼쳐보기가 되지 않는다는 것이다. 그래서 수정은 더 해줘야겠지만 2depth 이상의 데이터도 하위에 table 형식으로 출력해주는 함수를 간단히 만들어보았다. 개발하면서 개인적인 util 라이브러리를 만들어 사용하면 좋을 것 같다.

```javascript
  /**
	 * showTabularData : console debugger 창에 object 데이터를 테이블 형식으로 그려주는 함수
	 *
	 * @param data : console 창에 그려줄 object
	 * @param dataName : 그룹지어질 최상위 object 명
	 */
	 table : function(data, dataName){
 		console.group("***** SHOW TABULAR DATA *****");
 		if(typeof(data) === "object" && data != null && !(data.length != undefined && data.length == 0) ){
 			var title = dataName;
 			var showData = data;
 			if(data.length != undefined) {
 				title += " (length:" + data.length + ")";
 			} else if(data.length === 1) {
 				showData = data[0];
 			}
 			console.group(title);
 			console.table(showData);
 		} else {
 			console.log("There is no object data.");
 			console.log(data);
 		}

 		var func = function(data, dataName){		
 			for (var key in data) {
 				if(typeof(data[key]) === "object" && data[key] != null && !(data[key].length != null && data[key].length != undefined && data[key].length == 0)){
 					var title = key;
 					var showData = data[key];
 					if(data[key].length != undefined) {
 						title += " (length:" + data[key].length + ")";
 					} else if(data[key].length === 1) {
 						showData = showData[0];
 					}					
 					console.groupCollapsed(title);
 					console.table(showData);										

 					func(data[key], key);
 				}
 			}
 			console.groupEnd();
 		}		

 		func(data, dataName);
 		console.groupEnd();
 		console.groupEnd();
 	}
```
