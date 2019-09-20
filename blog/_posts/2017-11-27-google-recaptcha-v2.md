---
layout: post
title: '[Google] 구글 reCAPTCHA v2 적용하기'
author: hyesoo.shin
date: 2017-11-27 18:30
tags: [javascript, Google]
image: /files/covers/first_post.jpg
comments : true
---

# 구글 reCAPTCHA v2 적용하기

## 1. API Key 발급받기
* API key 발급 URL
  * http://www.google.com/recaptcha/admin
* reCAPTCHA를 이용할 사이트 도메인 등록
  * Label 입력 : 사이트에 대한 알아보기 쉬운 이름을 등록한다.
  * 리캡차 type 선택 : 가장 기본인 reCAPTCHA V2, 이미지 선택 이 없는 Invisible reCAPTCHA, Android 네이티브 용인 reCAPTCHA Android 세개의 type 중 하나를 선택한다.
  * Domains 입력 :실제 이용할 사이트의 도메인 주소를 입력한다
  * 리캡차 서비스에 동의하기 체크 : 무조건 동의를 해야 서비스 이용이 가능하다

* 참고 이미지

![theme:dark](/assets/images/post/2017-11-27-google-recaptcha-v2/20171127_grv2_full.jpg)


## 2. 소스 삽입하기

### Step1. Client side integration
소스를 삽입하는 방법에는 두가지가 있다.
1. Automatically render the widget
2. Explicitly render the widget

#### 1. Automatically render the widget
1번이 더 간단한 방법으로, 단순 히 recapcha 소스 파일을 받아오는 스크립트를 삽입해주고, g-recaptcha 라는 class가 들어간 dev 엘리먼트를 추가해 주는 것이다.
```html
<html>
  <head>
    <title>reCAPTCHA demo: Simple page</title>
     <script src="https://www.google.com/recaptcha/api.js" async defer></script>
  </head>
  <body>
    <form action="?" method="POST">
      <div class="g-recaptcha" data-sitekey="your_site_key"></div>
      <br/>
      <input type="submit" value="Submit">
    </form>
  </body>
</html>
```
#### 2. Explicitly render the widget
그러나, 스크립트를 받아 온 뒤 원하는 callback 함수를 실행할 수 있는 등 2번이 좀 더 확실하고 정교한 작업을 할 수 있을 것으로 보여 2번으로 접근하기로 하였다.

1. callback 함수를 정의해준다. (이 함수는 recaptcha와 관련된 api.js 소스 파일이 모두 load된 후 실행되므로 더욱 확실하다고 볼 수 있다.)
```html
<script type="text/javascript">
      var onloadCallback = function() {
          alert("grecaptcha is ready!");
      };
</script>
```

2. 자바스크립트 소스를 넣는다. callback 함수는 onload 파라미터의 값으로 넣어주고 render라는 파라미터에 explicit 라는 값을 넣어 명시적인 호출이라는 것을 알려줘야한다.
```html
<script src="https://www.google.com/recaptcha/api.js?onload=onloadCallback&render=explicit" async defer>
</script>
```

위에 있는 소스는 공식 사이트에 나와있는 소스다. 나는 실제로 스크립트를 받아온 후 콜백함수에서 리캡차를 랜딩하도록 수정해 보았다.(Sitekey는 공유되면 안되어서 임의의 값으로 표기하였다.)
```html
<script type="text/javascript">
	var verifyCallback = function(result) {
		console.log(">>>>> verifyCallback", result);
		if(result != null)
			$("#reCAPTCHA_result").html("success!!!!");
	};
	var onloadCallback = function() {
		grecaptcha.render('reCAPTCHA', {
			'sitekey'	: '6LdDqjoUAAAAAXXXXXXXXXXXXXXXXXXXXXXXXXXX',
			'callback' : verifyCallback
		});
	};
</script>
<script src="https://www.google.com/recaptcha/api.js?onload=onloadCallback&render=explicit" async defer></script>

<div id="reCAPTCHA"></div>
<div id="reCAPTCHA_result"></div>
```

위와 같이만 넣어도 리캡차가 수행된다. grecaptcha.render함수에 파라미터로 리캡차 영역이 추가될 div 앨리먼트의 id를 넣어준다. 여기서는 reCAPTCHA가 해당 id이고 그 div영역에 리캡차관련 iframe이 추가되면서 기능이 실행된다.
* 결과

  * 첫 실행 화면

  ![theme:dark](/assets/images/post/2017-11-27-google-recaptcha-v2/20171127_grv2_default1.jpg)

  * 로봇이 아닙니다 체크 후
  
  ![theme:dark](/assets/images/post/2017-11-27-google-recaptcha-v2/20171127_grv2_default2.jpg)
  * 이미지 모두 선택 후

![theme:dark](/assets/images/post/2017-11-27-google-recaptcha-v2/20171127_grv2_default3.jpg)

그리고 sitekey와 함께 callback 함수를 파라미터에 추가해 사용자 검증 결과를 리턴 받을 수 있다. 그 값은 아래와 같은 토큰 값을 가지고 있다. 이 토큰 값이 리턴된 것으로 검증 Step1이 끝나게된다.

```
03AO6mBfwNH1UYrRUwau8AuN51TRNHJRtE7SFJOXrwfLgr6zan5B_D2p010ZqzBnjru0f70YmXGqHaPqrjb69Xa4gy9XT3fgXcG95_g48rTB7PNMXMHyHcquWnwWQAvwREtJK74Ar9-RZfl_ICOWRbxMr9JqjTiExgl4edh68hPYfUrlQjTSyESLkGqSzqjKI9e6BjpnIjRrSWrO2dEJdYjGJL95...
```

그 외에 grecaptcha.render에 넣을 수 있는 parameter에는 몇 가지가 더 있다.
* theme : dark

  ![theme:dark](/assets/images/post/2017-11-27-google-recaptcha-v2/20171127_grv2_theme_dark.jpg)

* typeof : audio

  ![theme:dark](/assets/images/post/2017-11-27-google-recaptcha-v2/20171127_grv2_type_audio.jpg)
* size : compact

  ![theme:dark](/assets/images/post/2017-11-27-google-recaptcha-v2/20171127_grv2_size_compact.jpg)


### Step2. Server side integration
그리고 이 것만으로는 안전하지 않다고 보고 Step2에서는 API를 통해 Server-Side에서 한번 더 점검을 하게된다. Step1에서 리턴 받은 토큰 값과 secretkey를 API로 보내 한번 더 검증 받는 것으로 보인다. 하지만, 나는 이번에 v2는 분석이고 실제로 invisible reCAPTCHA를 적용해볼 것이기 때문에 Step2는 진행하지 않았다.

* API URL : https://www.google.com/recaptcha/api/siteverify
* 필수 parameter
  * secret : step1에서 발급받은 secket key
  * response : step1에서 받은 Token 값
* response
  * success : true/false

#### ※ 참고사이트
* [Developer's Guide](https://developers.google.com/recaptcha/old/intro)
* [Frequently asked questions](https://developers.google.com/recaptcha/docs/faq)
