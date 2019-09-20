---
layout: post
title: '[Struts] 스트럿츠 버전 확인 하는 방법'
author: hyesoo.shin
date: 2018-05-10 11:30
tags: [Struts]
image: /files/covers/first_post.jpg
comments : true
---

# 스트럿츠 버전 확인 하는 방법



## 1. struts.jar 파일 찾기
내 소스에서는 workspace\\_WC\lib\ 경로 안에 있었다.

## 2. struts.jar 압축해제하기

## 3. struts\META-INF\ 경로 안에 있는 MANIFEST.MF 파일 열기

## 4. Specification-Version 확인하기
오픈된 파일에서 Specification-Version으로 표기되어 있는 것이 Struts 버전이라고 한다.
내가 사용하고 있는 Struts는 1.2.9 버전이였다.
```
Manifest-Version: 1.0
Ant-Version: Apache Ant 1.6.1
Created-By: 1.3.1_04-b02 (Sun Microsystems Inc.)
Extension-Name: Struts Framework
Specification-Title: Struts Framework
Specification-Vendor: The Apache Software Foundation
Specification-Version: 1.2.9
Implementation-Title: Struts Framework
Implementation-Vendor: The Apache Software Foundation
Implementation-Vendor-Id: org.apache
Implementation-Version: 1.2.9
Class-Path:  commons-beanutils.jar commons-digester.jar commons-fileup
 load.jar commons-logging.jar commons-validator.jar jakarta-oro.jar
```
