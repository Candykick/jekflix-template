---
date: 2021-02-22 22:15:10
layout: post
title: "앞으로 쓸 글들"
subtitle:
description: "앞으로 쓸 예정인 글 목록"
image: "https://images.unsplash.com/photo-1613984431576-0beeedf617b4?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1510&q=80"
optimized_image: "https://images.unsplash.com/photo-1613984431576-0beeedf617b4?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1510&q=80"
category: androidkr
tags:
- android
- plan
author: candykick
---

앞으로 작성할 예정인 글들을 참고자료와 함께 정리해보았습니다. 시간 날 때마다 천천히 공부하면서 정리할 예정입니다. 이는 이번 Ourstock 앱을 개발하면서 얻은 지식들을 정리하고, 더 체계적으로 공부하기 위함입니다. (사실 분야별로 살펴본 자료가 1개 이상인데, 몇 개는 링크를 못 찾고 있는 상황입니다. 찾으면 다시 정리할 예정입니다.)

1. Koin Library
   - [Koin 잘 사용하기](https://medium.com/hongbeomi-dev/koin-%EC%9E%98-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-5a96a8bb94d3)
   - [Koin으로 Dialog 사용하기](https://myung6024.tistory.com/22)
2. 비동기 처리
3. LiveData
   - [LiveData vs ObservableField 무엇을 써야 할까?](https://seunghyun.in/android/5/)
4. LiveData with Event
   - [[안드로이드]MVVM과 LiveData 조합 시 겪을 수 있는 이슈와 해결책](https://vagabond95.me/posts/live-data-with-event-issue/)
   - [SingleLiveEvent to help you work with LiveData and events](https://proandroiddev.com/singleliveevent-to-help-you-work-with-livedata-and-events-5ac519989c70)
   - [SingleLiveEvent Code Sample](https://github.com/android/architecture-samples/blob/dev-todo-mvvm-live/todoapp/app/src/main/java/com/example/android/architecture/blueprints/todoapp/SingleLiveEvent.java)
5. 코루틴
   - ['날고싶은개발자'님의 코루틴 관련 칼럼들](https://jaejong.tistory.com/66)
   - [안드로이드 개발자 문서](https://developer.android.com/kotlin/coroutines?hl=ko)
6. MVVM 패턴 시리즈
   - [AAC를 사용하여 MVVM Architecture를 적용한 안드로이드 앱 만들기](https://medium.com/hongbeomi-dev/aac를-사용하여-mvvm-pattern을-구현한-안드로이드-앱-만들기-1d6d73689bd0)
7. MVVM에서 Forms의 유효성 검사를 진행하는 방법
   - '어플(앱)을 연구하는 사람들-안드로이드,IOS,개발자' 방의 방장 Chu님의 답변 : editText 유효성 검사를 databinding 쓰시면 bindingadapter로 빼셔서 viewmodel 같이 넘기셔서 활성 비활성화 시켜줘도 되고… 너무 다양해용!
     저라면… 바인딩 어댑터로 유효성 검사해서 통과 비통과 livedata로 가지고 있다가… 변경되는거 button 에 알아서 인식하게 바꿀거 같긴해용… 코딩에 정답은 항상 없습니당
   - '어플(앱)을 연구하는 사람들-안드로이드,IOS,개발자' 방의 funct7님의 답변 : 이건 stack overflow 검색하셔도 나오지만 의견이 갈립니다 정답 없는 문제
     영어 가능하시면 보셔도 좋아요. 결론은 없고 그냥 각자 의견 게제인데, wpf 시절부터 나오던 얘기라 전 그냥 이런 얘기가 나오는 자체가 mvvm이 별로여서라고 보는 입장입니다 ㅋㅋ
     앱 안에서 일관성만 있으면 어느 쪽이든 상관없는 것 같습니다
   - [Should the MVVM ViewModel perform type conversion/validation?](https://stackoverflow.com/questions/1334126/should-the-mvvm-viewmodel-perform-type-conversion-validation)
   - [How to implement validation using ViewModel and Databinding?](https://stackoverflow.com/questions/52371374/how-to-implement-validation-using-viewmodel-and-databinding)
8. 애니메이션(기본 애니메이션, LottieAnimationView)
9. 코틀린의 키워드들(it, this 등. 또 뭐 있더라?)
10. EditText 응용 : 이름, 전화번호, 금액, 한글금액 등등.
    - [Phone number formatting an EditText in Android](https://stackoverflow.com/questions/15647327/phone-number-formatting-an-edittext-in-android)
    - [Format EditText view for phone numbers](https://stackoverflow.com/questions/14692764/format-edittext-view-for-phone-numbers)
    - [[#1][안드로이드] EDITTEXT에 연락처 입력 시 자동 하이픈(-) 설정하기](http://dailyddubby.blogspot.com/2018/02/1-edittext.html)
11. 양방향 데이터 바인딩
    - [[Android] 안드로이드 양방향 데이터 바인딩 (2 way data binidng) 구현하기](https://salix97.tistory.com/276)

12. TextInputLayout의 색상 변경 및 테마 적용하기
    - [[안드로이드/Android\] TextInputLayout - 속성변경(밑줄 색, 커서 색, 메세지 등)](https://thgus13.tistory.com/24)