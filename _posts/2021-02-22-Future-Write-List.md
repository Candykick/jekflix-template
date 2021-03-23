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
   - [How to add a Fragment's argument item to Koin dependency graph?](https://stackoverflow.com/questions/56436705/how-to-add-a-fragments-argument-item-to-koin-dependency-graph)
   - [How to share same instance of ViewModel between Activities using Koin DI?](https://stackoverflow.com/questions/57077784/how-to-share-same-instance-of-viewmodel-between-activities-using-koin-di)
   - [How to share same instance of ViewModel between Activities ? #534](https://github.com/InsertKoinIO/koin/issues/534)
     -> Koin + MVVM 구조에서 Activity끼리 공통된 객체를 사용해야 하는 경우 어떻게 구현할지에 대한 의견. 윗 글까지 합쳐서 정리하면, ViewModel은 lifecycle을 가지고 있기 때문에 viewmodel 키워드를 singleton으로 변환해서는 안 되고, 대신 repository가 singleton이니 여기서 공통된 객체를 관리하라고 조언하고 있다.
   
2. 비동기 처리

3. LiveData
   
   - [LiveData vs ObservableField 무엇을 써야 할까?](https://seunghyun.in/android/5/)
   - [When to use MutableLiveData and LiveData](https://stackoverflow.com/questions/55914752/when-to-use-mutablelivedata-and-livedata)
     -> LiveData와 MutableLiveData의 차이.
   - [When to use emit() instead of postValue when using livedata with coroutines](https://stackoverflow.com/questions/61942845/when-to-use-emit-instead-of-postvalue-when-using-livedata-with-coroutines)
     -> emit() 함수와 postValue() 함수를 써야 하는 경우의 구분인데, 솔직히 이 글은 그렇게 큰 도움이 안 됬음.
   
4. LiveData with Event
   - [[안드로이드]MVVM과 LiveData 조합 시 겪을 수 있는 이슈와 해결책](https://vagabond95.me/posts/live-data-with-event-issue/)
   - [SingleLiveEvent to help you work with LiveData and events](https://proandroiddev.com/singleliveevent-to-help-you-work-with-livedata-and-events-5ac519989c70)
   - [SingleLiveEvent Code Sample](https://github.com/android/architecture-samples/blob/dev-todo-mvvm-live/todoapp/app/src/main/java/com/example/android/architecture/blueprints/todoapp/SingleLiveEvent.java)
   - 질문 : View에 연결해서 사용하지 않는 객체들도 ViewModel 안에선 LiveData로 사용해야 하는가? 아니면 전역 변수로 써도 문제없는 건가?
   
5. 코루틴
   - ['날고싶은개발자'님의 코루틴 관련 칼럼들](https://jaejong.tistory.com/66)
   - [안드로이드 개발자 문서](https://developer.android.com/kotlin/coroutines?hl=ko)
   
6. MVVM 패턴 시리즈

   - [AAC를 사용하여 MVVM Architecture를 적용한 안드로이드 앱 만들기](https://medium.com/hongbeomi-dev/aac를-사용하여-mvvm-pattern을-구현한-안드로이드-앱-만들기-1d6d73689bd0)
   - [MVVM 학습하기 좋은 글 끄적끄적 (LiveData, MVVM, DataBinding)](https://terry-some.tistory.com/28)
     -> MVVM에서 ViewModel이 여러 개의 View를 가질 수 있다는 것은 <strong>Fragment 한정</strong>이다!!! 즉, Activity-ViewModel은 여전히 1:1이 맞다는 말.
   - [RecyclerView.Adapter 어떻게 접근하면 좋을까? - ViewModel](https://thdev.tech/android/2018/01/31/Recycler-Adapter-Distinguish/)
     ->  MVVM에서 RecyclerView 사용 시, Adapter의 역할을 최소화하고, MVVM의 책임을 늘린 방식. 이를 통해 Adapter의 재사용성을 확보할 수 있다.
     -> ViewHolder부터 시작해서, Adapter까지의 재사용성을 정말 신경 많이 쓴 멋진 코드. 근데 어렵다.
   - [(android) MVVM 3. RecyclerView binding](https://apt-info.github.io/개발/android-data-binding-recyclerview/)
     -> Databinding을 고려한 코드. DiffUtil 사용법이 잘 나와 있다. 다만 ViewModel이 구식으로 짜여져 있어서 아쉬움.(LiveData가 빠져 있음.)

7. MVVM에서 Forms의 유효성 검사를 진행하는 방법
   - '어플(앱)을 연구하는 사람들-안드로이드,IOS,개발자' 방의 방장 Chu님의 답변 : editText 유효성 검사를 databinding 쓰시면 bindingadapter로 빼셔서 viewmodel 같이 넘기셔서 활성 비활성화 시켜줘도 되고… 너무 다양해용!
     저라면… 바인딩 어댑터로 유효성 검사해서 통과 비통과 livedata로 가지고 있다가… 변경되는거 button 에 알아서 인식하게 바꿀거 같긴해용… 코딩에 정답은 항상 없습니당
   - '어플(앱)을 연구하는 사람들-안드로이드,IOS,개발자' 방의 funct7님의 답변 : 이건 stack overflow 검색하셔도 나오지만 의견이 갈립니다 정답 없는 문제
     영어 가능하시면 보셔도 좋아요. 결론은 없고 그냥 각자 의견 게제인데, wpf 시절부터 나오던 얘기라 전 그냥 이런 얘기가 나오는 자체가 mvvm이 별로여서라고 보는 입장입니다 ㅋㅋ
     앱 안에서 일관성만 있으면 어느 쪽이든 상관없는 것 같습니다
   - [Should the MVVM ViewModel perform type conversion/validation?](https://stackoverflow.com/questions/1334126/should-the-mvvm-viewmodel-perform-type-conversion-validation)
   - [How to implement validation using ViewModel and Databinding?](https://stackoverflow.com/questions/52371374/how-to-implement-validation-using-viewmodel-and-databinding)
   
8. ViewModel에서 context 안전하게 사용하기
   : 사실 ViewModel에서 액티비티나 프래그먼트의 context를 참조하는 것은 피해야 한다. 다만 그게 필요한 경우가 있을 수 있으니... (사실 이론적으로 생각해보면, 화면에 대한 1:1 의존성을 피하기 위해 탄생한 게 ViewModel 방식이었으므로, Activity/Fragment의 context를 가지는 ViewModel은 이론적으로 잘못된 것이 아닐까 싶다.)

   - [[코틀린] ViewModel 에서 context 필요로 할 때 해결방법](https://youngest-programming.tistory.com/327)
     -> 의존성 주입으로 context를 넣지 못하는 경우.

   - [How to get Context in Android MVVM ViewModel](https://stackoverflow.com/questions/51451819/how-to-get-context-in-android-mvvm-viewmodel)
     -> applicationContext가 필요한 경우 사용할 수 있는 방법 1. 같은 ViewModel을 2개 이상의 Activity/Fragment에서 사용한다면 context를 파라미터로 주입받는 게 이상한 작동을 야기할 수 있다. (ViewModel은 singleton이고, 해당 singleton은 맨 처음 화면에 대한 context를 가지고 있을 것이기 때문. 근데 이게 맞나?)

   - [[kotlin] global application context](http://susemi99.kr/5149/)

     [[Kotlin] [Android] Global Application Context](https://kentakang.com/150)
     -> applicationContext가 필요한 경우 사용할 수 있는 또 다른 방법 2. 위 방법의 경우, applicationContext를 파라미터로 받아야 하기 때문에 직관적?이지 않다는 문제가 있다. 따라서 이 방식을 사용하는 것도 괜찮다.

9. 애니메이션(기본 애니메이션, LottieAnimationView)

10. 코틀린의 키워드들(it, this 등. 또 뭐 있더라?)

    - [코틀린 제네릭, in? out?](https://medium.com/mj-studio/코틀린-제네릭-in-out-3b809869610e)

11. EditText 응용 : 이름, 전화번호, 금액, 한글금액 등등.
    - [Phone number formatting an EditText in Android](https://stackoverflow.com/questions/15647327/phone-number-formatting-an-edittext-in-android)
    - [Format EditText view for phone numbers](https://stackoverflow.com/questions/14692764/format-edittext-view-for-phone-numbers)
    - [[#1][안드로이드] EDITTEXT에 연락처 입력 시 자동 하이픈(-) 설정하기](http://dailyddubby.blogspot.com/2018/02/1-edittext.html)

12. 양방향 데이터 바인딩

    - [[Android] 안드로이드 양방향 데이터 바인딩 (2 way data binidng) 구현하기](https://salix97.tistory.com/276)

13. TextInputLayout의 색상 변경 및 테마 적용하기

    - [[안드로이드/Android\] TextInputLayout - 속성변경(밑줄 색, 커서 색, 메세지 등)](https://thgus13.tistory.com/24)

14. 콜백 함수 사용하기

    - [Kotlin: how to pass a function as parameter to another?](https://stackoverflow.com/questions/16120697/kotlin-how-to-pass-a-function-as-parameter-to-another)
    
15. ActivityResultContract가 deprecated되면서 생긴 변화, ActivityResultContract로 migration하기

    - [새로운 API ActivityResultContract로 Migration](https://pluu.github.io/blog/android/2020/05/01/migation-activity-result/)
    - [registerForActivityResult 를 알아보자](https://bacassf.tistory.com/104)
    - [활동으로부터 결과 가져오기 (Android 공식 문서)](https://developer.android.com/training/basics/intents/result?hl=ko#kotlin)
    
16. RecyclerView 최상단에서 당겨서 새로고침 구현 / 최하단 데이터 추가 로딩 구현

    - [[Android] 당겨서 새로고침 간단하게 구현하기](https://medium.com/@bluesh55/android-당겨서-새로고침-간단하게-구현하기-a42846c14c23)
    - [[안드로이드] 리사이클러뷰 , 스와이프리프레쉬아웃 함께 사용 ( recyclerview, SwipeRefreshLayout)](https://youngest-programming.tistory.com/105)

17. Retrofit2에서 String Respose를 얻어오고 싶은 경우
    : 우선 String으로 결과값을 얻어온 뒤, 형태나 응답값 등에 따라 다른 형태의 객체로 변환할 경우 필요하다.
    [How to get string response from Retrofit2?](https://stackoverflow.com/questions/36523972/how-to-get-string-response-from-retrofit2)
    
18. BaseFragment with ViewDataBinding+ViewModel
    [How to make BaseFragment with View Binding](https://stackoverflow.com/questions/64819181/how-to-make-basefragment-with-view-binding)
    
19. 
    안드로이드에서 Access token을 안전하게 저장하고 관리하는 방법 : 의외로 SharedPreference를 써도 큰 문제가 없다. 근데 왜?
    [How to securely store access token and secret in Android?](https://stackoverflow.com/questions/10161266/how-to-securely-store-access-token-and-secret-in-android)