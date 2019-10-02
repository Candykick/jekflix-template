---
date: 2019-10-02 16:36:44
layout: post
title: "안드로이드에서 카카오 로그인 API 적용하기 (1)"
subtitle: "카카오 개발자 사이트에 앱 등록하고 설정하기"
description: 안드로이드 카카오 로그인 API 사용법 및 예제
image: http://drive.google.com/uc?export=view&id=1OGKBT2VJmihLc71CQTsPxkiGf3W7QB0B
optimized_image:
category: androidkr
tags:
- android
- kakaologin
author: candykick
---
 일단, 카카오 로그인을 적용하려면 카카오 개발자 페이지에 로그인해서, 내가 만들 앱을 등록해야 한다. 사실 이 과정은 기업에서 제공하는 API를 사용하려면 거의 거치는 과정이기도 하다.(카카오뿐만 아니라 네이버, 구글 API같은 걸 사용할 때도 이러한 과정을 거친다.)

우선, [카카오 개발자 페이지](https://developers.kakao.com)에 들어가보자.

 ﻿들어가서, 로그인부터 하자. 로그인을 하고 나면, 아래와 같은 화면이 나올 것이다. (필자는 이미 만든 앱이 몇 개 있어서 '전체 애플리케이션'에 몇 개의 앱이 나오는 것을 확인할 수 있다.) 여기서 '앱 만들기' 버튼을 클릭!

![placeholder](http://drive.google.com/uc?export=view&id=18ktNRQnSuQ_-d7GuyRy6rJ9VA97QRDnl "Large example image")

 아이콘은 뭐, 있으면 올리시면 되지만, 굳이 안 올려도 된다. 이름도 본인이 알아볼 수 있게만 설정하고 '앱 만들기' 버튼을 클릭한다. (참고로 이름에 kakao가 들어가면 생성이 안 된다.)

![placeholder](http://drive.google.com/uc?export=view&id=1yvq37JcRegwIi25uV8l20oAES5nlKfzE "Large example image")

그럼 아래와 같은 창이 나온다. 여기서 우리가 주목해야 할 것은 '네이티브 키'이다. 이 값을 이용해서 카카오 API와 통신하기 때문이다. 
이제 여기다가 내가 만들 앱을 등록해보자. 왼쪽의 '개요'를 클릭하자.

﻿![placeholder](http://drive.google.com/uc?export=view&id=1w8iOUWX-JRgioOAATBqs21I2xzgex9Sh "Large example image")

가운데 '앱 정보' 옆의 '설정' 버튼을 누른다.

![placeholder](http://drive.google.com/uc?export=view&id=1K10MwTWIzixxfAGfhQaOWgUCADGgrjx8 "Large example image")

'플랫폼 추가' 버튼을 클릭!

![placeholder](http://drive.google.com/uc?export=view&id=1izunOOuDqo9kkS5ZwCkESm0LhpdQQ8KJ "Large example image")

'Android'를 선택하고

![placeholder](http://drive.google.com/uc?export=view&id=1YQ9Lo8I8IBCp6hh1Fu2HrUM7rz7Arbrb "Large example image")

패키지명을 입력한다. 마켓 URL은 패키지명을 입력하면 자동으로 추가된다.  
(※참고로 앱을 구글 플레이스토어에 등록하지 않아도 마켓 URL은 추가되니, 상관하지 않아도 된다.)

﻿![placeholder](http://drive.google.com/uc?export=view&id=1EFUNndb_ebTPUR1r4MTNCJ0aGgA9ERTs "Large example image")

만약 패키지명이 어디 있는지 모른다면, 본인이 만들고 있는 안드로이드 스튜디오 프로젝트를 켠 뒤, 자바 소스 코드의 맨 위쪽 부분을 보면 된다. package 뒷부분이 패키지명이다.

![placeholder](http://drive.google.com/uc?export=view&id=1h5-gK_q1Cvxmdrqhb0G47TQOHMgEz7pP "Large example image")

'추가' 버튼을 누르면, 아래와 같이 나온다. 근데, 문제가 있다. 키 해시도 등록해야 한다.

![placeholder](http://drive.google.com/uc?export=view&id=1RTCOFqiqBrqtWZivpuVdIAXJ2fANpYU1 "Large example image")

키 해시가 뭐냐고? 보통 해시키(Hash Key)라고 불리는데, 안드로이드 개발 환경에서 가지고 있는 인증서에 대한 해쉬값이다. 쉽게 말해서, 각 개발자에 대한 고유한 키값이라고 생각하면 된다. 키 해시를 등록하지 않으면 카카오 API를 앱에서 호출할 수 없다.  
키 해시를 가져오는 방법은 다양하다. 그 중에서, 개인적으로는 아래 방법이 가장 간편하다고 생각한다. [이 블로그](https://wodonggun.github.io/wodonggun.github.io/android/Android-hash-key.html)를 참조해서 해시키를 가져오자.

﻿가져온 해시키를 입력한다.

![placeholder](http://drive.google.com/uc?export=view&id=1BN8YLkhZ7KnfJaglsyIQkPF6H_OezniS "Large example image")

그러면, 이제는 사용자 정보를 가져오기 위한 설정을 해보자.
<br><br><br>

# 가져올 사용자 정보 설정하기

왼쪽의 '설정'에서 '사용자 관리'를 클릭하자.

![placeholder](http://drive.google.com/uc?export=view&id=1LQ5MxEluROwAyzXt17Xfet9-rmtPn0vB "Large example image")

사용자 관리 스위치(?)가 OFF로 설정되어 있다. 스위치를 누르자.

![placeholder](http://drive.google.com/uc?export=view&id=1X3-ZNDpiPR1drY6dippiwqfyk5nLZZ1F "Large example image")

그러면 아래와 같이 '동의항목'이라는 것이 나온다.

![placeholder](http://drive.google.com/uc?export=view&id=1j4R7HFqnizux7i6bfpjEz3l-JMkdIqS0 "Large example image")

여기에 대해서 잠깐 설명하겠다. <font color="#007AA6"><ins><strong>카카오 로그인 API는 닉네임/프로필 사진을 제외한 정보를 불러오려면 우선 카카오에 해당 정보를 받아오는 이유를 밝혀야 한다.</strong></ins></font> 또한, 그런 정보를 받아올 때 무조건 사용자의 동의를 받아야 한다.  
따라서, 동의항목 중에서 자신이 이용해야 할 유저 정보가 있다면, 해당 부분에 대해서는 수집목적을 밝혀야 한다.  
일단 여기서는 모든 정보를 받아오는 것으로 하고, 전부 '연결 시 선택'을 선택한 뒤 수집목적을 기재할 것이다. 여러분들은 필요한 부분에 대해서만 '연결 시 선택'이나 '이용 중 사용'을 선택하고, 수집목적을 밝히면 된다.

![placeholder](http://drive.google.com/uc?export=view&id=1ojcGvSyiPDUyXpwC8cBIHnfiH8cSO-09 "Large example image")

> '연결 시 선택'과 '이용 중 사용' 항목의 차이?


* <strong><font color="#FF9300">'연결 시 선택'은 앱에 최초로 로그인 할 때, 성별이나 나잇대 등의 특정 정보를 앱에 제공할지 여부를 사용자에게 물어본다는 것이다.</font></strong>
* <font color="#007433"><strong>반면 '이용 중 사용'은 앱을 이용하는 도중에 특정 정보가 필요한 경우, 그 때 해당 정보를 제공할지 여부를 사용자에게 물어본다는 의미이다.</strong></font>

즉 '이용 중 사용'을 선택하면 최초 로그인 시에는 특정 정보를 제공받을지 여부를 물어보지 않으며, 최소한의 권한만 사용자에게 허락받게 된다.  
필요한 정보에 대해서 수집목적을 입력하고 '저장' 버튼을 누르면, 아래처럼 나온다.

﻿![placeholder](http://drive.google.com/uc?export=view&id=1oUNxfEE-sl7Wk0ygupf70AyP9gFyGP9w "Large example image")

드디어 모든 준비가 끝났다. 다음 글에서는 안드로이드 스튜디오를 키고, 직접 개발에 착수할 것이다!
<br><br><br>

# ※ 추가로 알아두면 좋은 정보들

* <strong><font color="#FF9300">앱을 실제로 스토어에 출시할 때는 해시키를 추가로 등록해야 한다!</font></strong>  
해시키는 두 가지 종류가 있다. 앱을 디버깅할 때 쓰는 해시키가 있고, 앱을 릴리즈할 때 쓰는 해시키가 그것이다. <strong><ins>즉, 만약 여러분이 실제로 앱을 출시하게 된다면, 본인의 Key Store로 앱을 빌드한 뒤에, 빌드된 앱을 실행해서 릴리즈용 해시키를 가져오고, 이를 앱 등록 페이지에 추가해야 정상적인 로그인이 수행된다!</ins></strong>
참고로 앱을 등록하는 페이지에서 해시키는 여러 개를 등록할 수 있으니, 기존의 디버깅용 해시키를 굳이 지울 필요는 없다. 이 부분에 대해서는 추후 글에서 다시 설명할 예정이다.  
  
* <strong><font color="#007AA6">닉네임/프로필 사진을 제외한 정보를 받아올 때는 무조건 사용자 동의를 받아야 한다!</font></strong>  
예를 들어, 소개팅 앱을 개발한다고 해 보자. 그리고 해당 앱에서는 로그인 시 유저의 성별과 나잇대가 필요하다고 가정해 보자. 그런데, 사용자가 로그인하면서 나잇대 정보를 제공하는 것에는 찬성했지만 성별 정보를 제공하는 것을 거절한다면? 당연히 성별 정보는 넘어오지 않게 된다. 만약 이에 대한 프로세스를 구현해놓지 않는다면, 성별 정보가 없는 상태로 새 유저가 등록되거나, 넘어오는 정보가 부족해서 앱이 강제 종료되는 경우가 생길 것이다.  
<ins><strong>즉, 닉네임/프로필 사진을 제외한 정보가 필요한 경우, 사용자가 해당 정보를 제공하는 것을 거절하면 팝업창을 띄우거나, 로그인을 취소하는 등의 기능을 만들어둬야 한다.</strong></ins>  
이 부분에 대해서도 추후 글에서 따로 설명할 예정이다.  
  
(여담으로, 예전에는 특정 정보를 무조건 제공받아야 할 경우 '연결 시 필수'를 선택해서 로그인 시 해당 정보를 무조건 받아오게 할 수 있었다. 아마 개인정보 관련 이슈가 대두되면서 바뀐 거 같은데, 덕분에 개발자들의 일은 더욱 늘어나게 되었다 ㅠㅠ)
