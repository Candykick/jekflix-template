---
date: 2021-03-27 23:17:22
layout: post
title: "안드로이드 의존성 주입 라이브러리, Koin #1"
subtitle: "Koin은 무엇인가 + 기본적인 사용법"
description: "Android Koin #1"
image: "https://raw.githubusercontent.com/InsertKoinIO/koin/master/docs/img/koin_2.0.jpg"
optimized_image: "https://raw.githubusercontent.com/InsertKoinIO/koin/master/docs/img/koin_2.0.jpg"
category: androidkr
tags:
- android
- koin
- injection
- kotlin
- library
- DSL
author: candykick
---

  앱을 구조화하는 일은 중요하다. 특히 코드의 재사용성을 높이는 일은 그 무엇보다도 중요하다. 코드의 재사용성을 높이는 방법은 여러 가지가 있겠지만, 그 중에서도 각 코드 사이의 의존성을 줄이는 일이 참 중요하다. 왜냐하면, 한 쪽 코드가 바뀌었다고 해서 다른 쪽 코드가 바뀌어야 한다면, 모듈화/계층화가 제대로 이루어지지 않은 것이나 마찬가지이기 때문이다. 그래서 MVP, MVVM 같은 디자인 패턴이 등장하면서 코드의 의존성을 줄이기 위한 여러 라이브러리가 개발되었는데, 나는 그 중에서도 쉽고, 가장 많이 쓰이는 Koin을 사용하고 있다.



### Koin이란?

<b><font color="#D18063">Koin : 코틀린 의존성 주입 라이브러리</font></b><br>
  [Koin 사이트](https://insert-koin.io)에선 Koin을 'a smart Kotlin injection library to keep you focused on your app, not on your tools'라고 설명하고 있다. 즉, 코틀린에서 쓰이는 의존성 주입(injection)용 라이브러리로 볼 수 있다. 코틀린에서 쓰이는 만큼, 자바에선 사용하기가 어렵다. 근데 잠만, 의존성 주입은 뭐지?

<b><font color="909FA6">의존성 주입 : 하나의 객체가 다른 객체의 의존성을 제공하는 테크닉</font></b> (by [위키백과](https://ko.wikipedia.org/wiki/의존성_주입))<br>
  위키백과에서 설명하는 의존성 주입은 위와 같다. '의존성'이라는 단어부터가 참 정의내리기 애매한데, 위키백과에선 '의존성'을 "의존성은 예를 들어 서비스로 사용할 수 있는 객체이다. 클라이언트가 어떤 서비스를 사용할 것인지 지정하는 대신, 클라이언트에게 무슨 서비스를 사용할 것인지를 말해주는 것이다. 주입은 의존성(서비스)을 사용하려는 객체(클라이언트)로 전달하는 것을 의미한다." 라고 길게 설명하고 있다. 정확하다고 생각하지만, 동시에 이해하기 어려운 설명이기도 하다.<br>
  내가 이해한 <b><u>'의존성'</u></b>은 <b><u>한 객체의 생성 및 사용에 있어서, 다른 객체가 필요한 경우</u></b>를 말한다고 생각한다. 가장 간단하게 생각해볼 수 있는 건 한 객체 A의 생성자에 다른 객체 B가 들어가는 경우이다. 이런 경우, A는 B에 대해 의존성을 가지게 된다고 볼 수 있다. 만약 여러 함수에서 객체 A를 사용하고 있었는데, 객체 A의 생성자에 새로운 파라미터가 들어갔다고 생각해 보자. 이 경우, 원래대로라면 객체 A를 쓰는 모든 함수가 수정되어야 한다. 하지만 이는 너무 노가다에 가깝다. 이럴 때, 의존성 주입을 이용해서 객체 A의 생성을 파라미터 없이, 쉽게 할 수 있게 한다면? 이런 수정 작업 자체가 필요없어진다!! 즉, <b>의존성 주입을 쓰면 코드의 모듈화가 쉬워지고, 이로 인해 코드 간 연관성이 줄어들면서, 한 쪽 코드가 수정되더라도 다른 쪽 코드를 수정해야 하는 일 자체를 줄일 수 있다!!!</b>



### Koin은 주로 어디에 쓰이는가?

<b><font color="#D18063">: MVVM에서 많이 쓰인다!!!</font></b><br>
  다른 곳에서도 쓰이겠지만, 주로 MVVM에서 Model - ViewModel - View의 분리를 위해서 많이 쓴다.<br>
  정확히는, ViewModel - View의 의존성을 줄이기 위해서 ViewModel을 모듈화하는 데에 많이 쓰고, 또 ViewModel - Repository의 의존성을 줄이기 위해서 Repository (추가로 외부 API를 호출하는 통신 모듈들)를 모듈화하는 데에도 사용된다. 여기서도 MVVM에서의 Koin 사용법을 위주로 설명할 것이다.



### Koin 설치

(* 참고로 Koin v3는 Multiplatform이라고 표시되어 있는데, 아직 미완성인 것으로 보인다. 그래서 여기선 안정적인 Koin v2를 이용했다.)

1. [Koin Github Repository](https://github.com/InsertKoinIO/koin)에서 최신 버전을 확인한다. 이 글을 쓰는 날짜 기준으론 2.2.2가 최신 버전이다.

2. root 수준의 build.gradle을 수정한다.<br>
   아래와 같이 buildscript 안에 ext.koin_version으로 최신 버전을 지정해 주고,<br>
   repositories 안에 jcenter() 를 써준 뒤,<br>
   dependencies 안에 classpath "org.koin:koin-gradle-plugin:$koin_version" 를 쓰고,<br>
   맨 아랫줄에 apply plugin: 'koin' 을 써 주면 된다.

   ```json
   // Top-level build file where you can add configuration options common to all sub-projects/modules.
   buildscript {
   		...
       ext.koin_version = "2.2.2" // Koin Library 최신 버전
   
       repositories {
           ...
           jcenter()
       }
       dependencies {
   				...
           classpath "org.koin:koin-gradle-plugin:$koin_version"
       }
   }
   
   allprojects {
       repositories {
   				...
           jcenter()
       }
   }
   
   ...
   
   apply plugin: 'koin'
   ```

   3. app 수준의 build.gradle을 수정한다.<br>
      아래와 같이 dependencies에 아래의 내용들을 추가해주면 된다.

      ```json
      ...
      
      dependencies {
      	...
      	// Koin (의존성 주입 프레임워크)
        // 각각 위에서부터 코어, 유닛 테스트, Scope 생성-삭제 자동화, ViewModel, Fragment
        implementation "org.koin:koin-core:$koin_version" // Koin 코어: 필수로 넣어야 함
        testImplementation "org.koin:koin-test:$koin_version" // Koin 유닛 테스트
        implementation "org.koin:koin-androidx-scope:$koin_version" // Scope 생성-삭제 자동화
        implementation "org.koin:koin-androidx-viewmodel:$koin_version" // Koin ViewModel
        implementation "org.koin:koin-androidx-fragment:$koin_version" // Koin Fragment
      }
      ```

      

### Koin Module 만들기

```kotlin
val "Module 이름" = module {
    single<"Interface 이름"> { "Class 생성자"("parameter" = get()) }
}
```



