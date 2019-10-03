---
date: 2019-10-03 11:07:07
layout: post
title: "안드로이드에서 카카오 로그인 API 적용하기 (2)"
subtitle: "버튼 클릭 시 로그인 수행하기"
description: 안드로이드 카카오 로그인 API 사용법 및 예제
image: http://drive.google.com/uc?export=view&id=1sspzfOF4yuSf1zsC9BMLCXakiHvHgvyv
optimized_image:
category: androidkr
tags:
- android
- kakaologin
author: candykick
---
   ★전체 프로젝트 GitHub에 올려놨습니다! 1강~5강까지의 내용이 담겨 있습니다.  
   [카카오 로그인 프로젝트 보러가기](https://github.com/Candykick/KakaoLogin)


※ 이 글부터는 예제를 가지고 설명할 예정입니다. 여기서 만들 예제는 LoginActivity에서 카카오 로그인을 수행하면, MainActivity에서 로그인한 유저의 이름과 프로필 사진 URL을 보여주는 앱입니다. 다음과 같이 작동하는 간단한(?) 앱입니다.
![placeholder](http://drive.google.com/uc?export=view&id=1ZlqgewXDHtWKNJol9mP5DB09sliFY0M_ "Large example image")

# 내 프로젝트에 카카오 로그인 API 추가하기

※ 이 글은 카카오 개발자 사이트의 Android 개발가이드를 바탕으로 작성되었으며, 제가 실제로 개발하면서 겪었던 이슈에 대한 대응책도 같이 적혀 있습니다. 만약 카카오의 Android 개발가이드를 보고 싶으시다면, [Android에서 개발 시작하기](https://developers.kakao.com/docs/android/getting-started)나 [사용자 관리](https://developers.kakao.com/docs/android/user-management)를 참고하셔서 작업하시면 됩니다.﻿  

★ <ins><strong>최소 앱 작동사양이 안드로이드 SDK 버전 4.0 아이스크림 샌드위치(API 14) 이상이어야 카카오 로그인 API가 정상적으로 작동합니다.</strong></ins> 프로젝트를 생성할 때 API 14 이상을 선택하거나, build.gradle(Module:app)에서 minSdkVersion이 14 이상이 되도록 설정하시면 됩니다.﻿  
![placeholder](http://drive.google.com/uc?export=view&id=18W3O23yRa0RGc_CEp9y6WEH8pXMWv3cS "Large example image")

카카오 로그인 API를 이용하려면, 우선 내 프로젝트에 카카오 로그인 API를 추가해야 합니다. 아래 순서대로 작업하시면 됩니다.  
(* 여기서는 <ins><strong>안드로이드 스튜디오, Gradle 환경</strong></ins>을 중심으로 서술합니다. 다른 환경에서 개발 중이시라면, ['Android에서 개발 시작하기'](https://developers.kakao.com/docs/android/getting-started)를 참고하시기 바랍니다. 또한, 카카오 로그인 API 1.16.0 버전을 중심으로 서술하기 때문에, 추후 새 버전이 나오면 몇몇 사항이 바뀔 수 있습니다.)    

1) Gradle Scripts에서 build.gradle(Project: xxxxxxxxx)을 열고, 아래의 내용을 추가합니다.
```gradle
subprojects {
   repositories {
      mavenCentral()
      maven { 
        url 'http://devrepo.kakao.com:8088/nexus/content/groups/public/' 
      }
   }
}
```
![placeholder](http://drive.google.com/uc?export=view&id=1q-4Q7zCRgHsMGASEugWAb6fWU-ry7tBG "Large example image")

2) build.gradle(Module: app)을 열고, 'dependencies' 블럭 안에 아래의 내용을 추가합니다.  
(1.16.0은 API 버전으로, 2019년 2월 기준 가장 최신 버전입니다. 만약 개발하시는 날짜의 최신 버전을 알고 싶으시다면, [SDK 다운로드 페이지](https://developers.kakao.com/docs/sdk)에 들어가셔서 Android SDK 항목 아래의 [Full SDK Source & Samples for Gradle Project](https://developers.kakao.com/sdk/latest-android-sdk) 버전을 확인하시면 됩니다.)
```gradle
implementation 'com.kakao.sdk:usermgmt:1.16.0'
```
![placeholder](http://drive.google.com/uc?export=view&id=1uuiXmFwW-CAy9k9Mjun1rNIyzN56K3h0 "Large example image")
﻿
3) 상단의 Sync Now를 클릭해서 프로젝트를 Rebuild합니다. 만약 Sync Now가 안 나오면 상단 메뉴에서 [Build]-[Rebuild Project]를 클릭합니다.
![placeholder](http://drive.google.com/uc?export=view&id=1o_gDdV6_HIQGJ6rqDcZ9Tw56PMJKXUuM "Large example image")

4) res 폴더의 values 폴더를 선택하고, strings.xml 파일을 엽니다. 여기에 앱 생성시 발급된 네이티브 앱 키를 <resources> 태그 안에 다음과 같이 입력합니다. AAAAAAAAAAAAAAAAAAAAAA라고 표시된 자리에 발급받은 네이티브 앱 키를 넣으시면 됩니다.
```xml
<string name="kakao_app_key">AAAAAAAAAAAAAAAAAAAAAA</string>
```
![placeholder](http://drive.google.com/uc?export=view&id=1iAet1NUUcTPv3XIXDgDjyWlbswXKqFeS "Large example image")

네이티브 앱 키는 카카오 개발자 페이지의 '내 애플리케이션' 페이지에서 개발중인 앱을 선택하면 볼 수 있습니다.
![placeholder](http://drive.google.com/uc?export=view&id=1BCGGb4zDSMpI7bqJUL7yv8rONA7QlZkT "Large example image")

5) manifests를 클릭해서 Android Manifest 파일을 엽니다. 그리고 <manifest> 태그 안에 네트워크 권한을 설정하기 위해 인터넷 퍼미션을 입력합니다.
```xml
<uses-permission android:name="android.permission.INTERNET" />
```
그 다음, <application> 태그 안에 4)에서 입력한 네이티브 앱 키를 입력합니다.
```xml
<meta-data
   android:name="com.kakao.sdk.AppKey"
   android:value="@string/kakao_app_key" />
```
![placeholder](http://drive.google.com/uc?export=view&id=1EMKJSSEkzf6lopoZQJC_vmz27ogNqfV2 "Large example image")


여기까지 하면, 필요한 사전작업은 끝났습니다. 이제는 직접 자바 파일과 레이아웃 파일을 열고, 카카오 로그인 버튼을 추가하면 됩니다!

# 레이아웃에 카카오 로그인 버튼 추가하기
    ★ 이 강의에서는 LoginActivity에서 로그인을 수행하면, MainActivity에서 로그인 정보를 보여주는 방식으로 작업했습니다.


카카오 로그인 API는 카카오 로그인 버튼을 따로 제공하고 있습니다. 굳이 버튼을 만들고, 버튼에 이미지를 씌울 필요가 없다는 것이죠. (※물론, 본인이 직접 만든 버튼에 카카오 로그인을 연동하는 것도 가능합니다. 그 방법은 이 강의 맨 아래쪽에 적혀 있습니다.)  
카카오 로그인 버튼을 사용하는 방법은 다음과 같습니다.


1) 로그인 기능이 들어가는 레이아웃 파일을 엽니다. 여기서는 로그인 기능이 MainActivity에 들어가므로 activity_main.xml 파일을 열었습니다.
2) 아랫쪽의 Text를 눌러 xml 입력 화면으로 전환한 뒤, Layout 태그 안에서 "<Login"까지만 입력하면 <strong><ins><font color="#FF9300">com.kakao.usermgmt.LoginButton</font></ins></strong>이란 위젯이 나옵니다. 클릭하면 됩니다.
![placeholder](http://drive.google.com/uc?export=view&id=1Vzz-AkekQf_3ojSLTWQSW3Xm6Z9CEruy "Large example image")
3) 레이아웃 크기 및 위치를 입력합니다. GUI 환경(Design 탭)에서 하셔도 상관없습니다.
![placeholder](http://drive.google.com/uc?export=view&id=1AC4iIdW_MEif1cOSzWeQ3Lv8ASJbBfmR "Large example image")
위와 같이 화면 정가운데에 카카오 로그인 버튼이 표시됩니다.
![placeholder](http://drive.google.com/uc?export=view&id=1ojNsFKCA4g903znB-56noyenMCBDugPk "Large example image")
로그인 버튼 부분 xml 코드는 다음과 같습니다. 아랫쪽의 app:으로 시작하는 네 줄은 로그인 버튼의 위치를 정해주는 부분으로, 필수적인 코드는 아닙니다.
```xml
<com.kakao.usermgmt.LoginButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent"/>
```
로그인 버튼을 추가했으니, 이제는 자바 소스 코드를 작성해봅시다!

# 카카오 로그인을 처리하기 위한 밑작업 하기

로그인 버튼을 누르면 아래와 같은 카카오 로그인 창을 띄우고, 로그인 성공 시 MainActivity로 넘기는 것까지 해보겠습니다.


※예전 버전을 쓰시던 분들은 참고하세요!  
저도 이 포스팅을 쓰면서 알게 된 겁니다만, <strong><ins><font color="#FF0010">버전이 업그레이드되면서, 카카오 로그인 API의 함수 몇 개가 바뀌었습니다!</font></ins></strong> 저는 상당히 오래 된 버전인 1.2.2 버전으로 작업을 했었는데요, 그 이후 특정 버전에서 함수가 바뀐 것으로 보입니다. 함수가 바뀐 것뿐만 아니라, 몇몇 기능도 추가된 상황입니다. 만약 옛날 버전으로 작업하시는 분들이 계시다면 이 부분을 염두에 두시기 바랍니다.  
일단 여기서는 최신 버전(2019년 2월 기준)의 소스코드만을 올려 두겠습니다. 만약 예전 버전 코드가 필요하시다면, 댓글로 저한테 알려 주세요. 추가 포스팅으로 예전 소스코드도 올려 두도록 하겠습니다.

﻿
우선, 카카오 로그인 API를 정상적으로 사용하려면 Application을 상속받은 class를 하나 만들어야 합니다.  
App이란 이름의 Class를 하나 만드시고, 아래 소스 코드를 그대로 쓰시면 됩니다!  
다만, 만약 기존에 Application을 상속받은 class가 존재한다면, 그 class에 아래 소스코드에 있는 클래스 및 함수들을 적당한 위치에 넣으시면 됩니다.
```java
public class App extends Application {

    private static volatile App instance = null;

    private static class KakaoSDKAdapter extends KakaoAdapter {
    /**
    * Session Config에 대해서는 default값들이 존재한다.
    * 필요한 상황에서만 override해서 사용하면 됨.
    * @return Session의 설정값.
    */
    // 카카오 로그인 세션을 불러올 때의 설정값을 설정하는 부분.
        public ISessionConfig getSessionConfig() {

            return new ISessionConfig() {
                @Override
                public AuthType[] getAuthTypes() {
                    return new AuthType[] {AuthType.KAKAO_LOGIN_ALL};
                    /*로그인을 하는 방식을 지정하는 부분. AuthType로는 다음 네 가지 방식이 있다.
                    KAKAO_TALK: 카카오톡으로 로그인, KAKAO_STORY: 카카오스토리로 로그인, KAKAO_ACCOUNT: 웹뷰를    통한 로그인,
                    KAKAO_TALK_EXCLUDE_NATIVE_LOGIN: 카카오톡으로만 로그인+계정 없으면 계정생성 버튼 제공
                    KAKAO_LOGIN_ALL: 모든 로그인방식 사용 가능. 정확히는, 카카오톡이나 카카오스토리가 있으면 그 쪽으로 로그인 기능을 제공하고, 둘 다 없으면 웹뷰를 통한 로그인을 제공한다.
                    */

                }

                @Override
                public boolean isUsingWebviewTimer() {
                    return false;
                    // SDK 로그인시 사용되는 WebView에서 pause와 resume시에 Timer를 설정하여 CPU소모를 절약한다.
                    // true 를 리턴할경우 webview로그인을 사용하는 화면서 모든 webview에 onPause와 onResume 시에 Timer를 설정해 주어야 한다.
                    // 지정하지 않을 시 false로 설정된다.
                }

                @Override
                public boolean isSecureMode() {
                    return false;
                    // 로그인시 access token과 refresh token을 저장할 때의 암호화 여부를 결정한다.
                }

                @Override
                public ApprovalType getApprovalType() {
                    return ApprovalType.INDIVIDUAL;
                    // 일반 사용자가 아닌 Kakao와 제휴된 앱에서만 사용되는 값으로, 값을 채워주지 않을경우 ApprovalType.INDIVIDUAL 값을 사용하게 된다.
                }

                @Override
                public boolean isSaveFormData() {
                    return true;
                    // Kakao SDK 에서 사용되는 WebView에서 email 입력폼의 데이터를 저장할지 여부를 결정한다.
                    // true일 경우, 다음번에 다시 로그인 시 email 폼을 누르면 이전에 입력했던 이메일이 나타난다.
                }
            };
        }

        @Override
        public IApplicationConfig getApplicationConfig() {
            return new IApplicationConfig() {
                @Override
                public Context getApplicationContext() {
                    return App.getGlobalApplicationContext();
                }
            };
        }
    }

    public static App getGlobalApplicationContext() {
        if(instance == null) {
            throw new IllegalStateException("this application does not inherit com.kakao.GlobalApplication");
        }
        return instance;
    }

    @Override
    public void onCreate() {
        super.onCreate();
        instance = this;

        KakaoSDK.init(new KakaoSDKAdapter());
    }

    @Override
    public void onTerminate() {
        super.onTerminate();
        instance = null;
    }
}
```
﻿
<strong><ins><font color="#007AA6">ISessionConfig는 카카오 로그인과 관련된 설정을 저장하는 객체입니다.</font></ins></strong>  
일반적으론 저 위의 소스코드를 그대로 쓰셔도 됩니다만, 설정을 건드리고 싶으신 분들이 계실 수 있어서 ISessionConfig의 각 속성값에 대해서는 주석으로 따로 설명을 달아두었습니다. 더 자세한 내용은 [카카오 개발자 페이지](https://developers.kakao.com/docs/android/user-management#로그인-사용법)의 ISessionConfig 부분을 참조하시면 됩니다.﻿


이 작업이 끝났으면, manifest를 한 번 더 수정해야 합니다. application 태그 안 쪽에 <font color="#0000FF">android:name=</font><font color="#008000">".App"</font>이란 값을 추가해주시면 됩니다.﻿
![placeholder](http://drive.google.com/uc?export=view&id=1HSVzsWVsIPL9xRlTNV2_zmX5-zfEpRuB "Large example image")
<br><br><br>

# 카카오 로그인 작업 구현하기(LoginActivity 구현)

 위 작업이 끝났으면, 카카오 로그인 버튼이 있는 Activity(여기서는 LoginActivity)로 넘어갑시다. 여기서는 카카오 로그인 콜백 관련 작업을 처리하고, 로그인 시 어떤 작업을 해야 하는지를 코딩해주시면 됩니다. 위에서 언급한 대로 전체 소스 코드를 먼저 올리고, 하나하나 설명해 보겠습니다.
 ```java
 package woo171tm.blog.bloglogintestkakao;
 
 import android.content.Intent;
 import android.support.v7.app.AppCompatActivity;
 import android.os.Bundle;
 import android.widget.Toast;
 
 import com.kakao.auth.ISessionCallback;
 import com.kakao.auth.Session;
 import com.kakao.network.ErrorResult;
 import com.kakao.usermgmt.ApiErrorCode;
 import com.kakao.usermgmt.UserManagement;
 import com.kakao.usermgmt.callback.MeV2ResponseCallback;
 import com.kakao.usermgmt.response.MeV2Response;
 import com.kakao.util.exception.KakaoException;
 
 public class LoginActivity extends AppCompatActivity {
 
    private SessionCallback sessionCallback;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);
 
        sessionCallback = new SessionCallback();
        Session.getCurrentSession().addCallback(sessionCallback);
        Session.getCurrentSession().checkAndImplicitOpen();
    }
 
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if(Session.getCurrentSession().handleActivityResult(requestCode, resultCode, data)) {
            super.onActivityResult(requestCode, resultCode, data);
            return;
        }
    }
 
    @Override
    protected void onDestroy() {
        super.onDestroy();
        Session.getCurrentSession().removeCallback(sessionCallback);
    }
 
    private class SessionCallback implements ISessionCallback {
        @Override
        public void onSessionOpened() {
            UserManagement.getInstance().me(new MeV2ResponseCallback() {
                @Override
                public void onFailure(ErrorResult errorResult) {
                    int result = errorResult.getErrorCode();
 
                    if(result == ApiErrorCode.CLIENT_ERROR_CODE) {
                        Toast.makeText(getApplicationContext(), "네트워크 연결이 불안정합니다. 다시 시도해 주세요.", Toast.LENGTH_SHORT).show();
                        finish();
                    } else {
                        Toast.makeText(getApplicationContext(),"로그인 도중 오류가 발생했습니다: "+errorResult.getErrorMessage(),Toast.LENGTH_SHORT).show();
                    }
                }
 
                @Override
                public void onSessionClosed(ErrorResult errorResult) {
                    Toast.makeText(getApplicationContext(),"세션이 닫혔습니다. 다시 시도해 주세요: "+errorResult.getErrorMessage(),Toast.LENGTH_SHORT).show();
                }
 
                @Override
                public void onSuccess(MeV2Response result) {
                    Intent intent = new Intent(getApplicationContext(), MainActivity.class);
                    intent.putExtra("name", result.getNickname());
                    intent.putExtra("profile", result.getProfileImagePath());
                    startActivity(intent);
                    finish();
                }
            });
        }
 
        @Override
        public void onSessionOpenFailed(KakaoException e) {
            Toast.makeText(getApplicationContext(), "로그인 도중 오류가 발생했습니다. 인터넷 연결을 확인해주세요: "+e.toString(), Toast.LENGTH_SHORT).show();
        }
    }
 }
 ```

핵심은 역시 SessionCallback 부분이겠네요. 이 부분에 대해 설명하겠습니다.(*필요한 건 위에 다 구현되어 있으므로, 급하시다면 이 부분은 그냥 넘어가셔도 됩니다.)  
<strong><ins><font color="#007AA6">SessionCallback은 ISessionCallback을 상속받은 카카오 로그인 콜백입니다.</font></ins></strong> 기본적인 구조는 아래와 같습니다.

```java
private class SessionCallback implements ISessionCallback {
    @Override
    public void onSessionOpened() {
        //로그인 세션이 열렸을 때.
    }

    @Override
    public void onSessionOpenFailed(KakaoException e) {
        //로그인 세션이 정상적으로 열리지 않았을 때.
    }
}
```

두 가지 함수가 있죠. <strong><ins><font color="#007AA6">onSessionOpened는 로그인 버튼 클릭 시, 로그인 세션이 제대로 열렸을 경우 작동하는 함수입니다.</font></ins></strong>  
<strong><ins><font color="#B85C00">onSessionOpenFailed는 그 반대고요.</font></ins></strong> 참고로, 인터넷이 아예 연결되어 있지 않아도 onSessionOpenFailed가 작동합니다. 반면, 인터넷이 연결은 되어 있는데 불안정한 경우(와이파이 신호가 너무 약하거나 등등) onSessionOpened가 실행되죠.  
일단, onSessionOpenFailed에서 해 줄 것은 간단합니다. 통상적인 에러 처리가 그것이죠. 여기서는 로그인 도중 오류가 발생했다는 토스트를 하나 띄워줍니다. 필요하다면 앱을 종료시키는 등의 동작도 할 수 있겠죠.  
onSessionOpened에서 할 것은 로그인을 수행하고, 그 정보를 받아오는 것입니다. 또한, 로그인 도중 오류가 발생하면 그 오류를 처리해줘야 하죠. <strong><ins><font color="#007AA6">유저 정보를 받아오는 함수 이름은 me로, MeV2ResponseCallback을 필요로 합니다.</font></ins></strong> 이 부분의 구조는 아래와 같습니다.

```java
UserManagement.getInstance().me(new MeV2ResponseCallback() {
    @Override
    public void onFailure(ErrorResult errorResult) {
        //로그인에 실패했을 때. 인터넷 연결이 불안정한 경우도 여기에 해당한다.
    }

    @Override
    public void onSessionClosed(ErrorResult errorResult) {
        //로그인 도중 세션이 비정상적인 이유로 닫혔을 때
    }

    @Override
    public void onSuccess(MeV2Response result) {
        //로그인에 성공했을 때
    }
});
```

위의 구조를 그대로 onSessionOpened 블럭 안에 넣고, 3가지 경우에 대한 처리를 구현해주시면 됩니다.  
<strong><ins><font color="#FF0000">우선 onFailure는 로그인이 실패했을 때 호출되는 함수입니다.</font></ins></strong> 어떤 오류인지를 나타내는 ErrorResult값을 받아오죠. 보통은 에러가 발생했다는 토스트 메세지를 띄웁니다. 다만, 다른 종류의 에러는 거의 발생하지 않지만, 인터넷 연결과 관련한 오류는 생각보다 자주 나타나죠. 따라서, 인터넷 연결이 불안정한 경우라면 따로 "인터넷 연결이 불안정합니다."하고 알려주는 게 좋습니다. 그래서, 여기서는 onFailure를 다음과 같이 구현했습니다.

```java
@Override
public void onFailure(ErrorResult errorResult) {
    int result = errorResult.getErrorCode();

    if(result == ApiErrorCode.CLIENT_ERROR_CODE) {
        Toast.makeText(getApplicationContext(), "네트워크 연결이 불안정합니다. 다시 시도해 주세요.", Toast.LENGTH_SHORT).show();
        finish();
    } else {
        Toast.makeText(getApplicationContext(),"로그인 도중 오류가 발생했습니다: "+errorResult.getErrorMessage(),Toast.LENGTH_SHORT).show();
    }
}
```

ErrorResult 객체에서 오류 코드를 가져온 뒤(3번째 줄), 그 오류 코드가 CLIENT_ERROR_CODE이면 네트워크 연결 문제라는 토스트 메세지를 띄우고, 아니라면 에러 메세지를 포함한 오류 발생 토스트 메세지를 띄웁니다.  
다음으로, <strong><ins><font color="#B85C00">onSessionClosed는 로그인 도중 세션이 비정상적인 이유로 닫혔을 때 작동하는 함수입니다.</font></ins></strong> 사실 실제로 이 오류가 발생하는 경우는 거의 없습니다. 어쨎든 비정상적인 작동을 하는 셈이니, 여기서는 그냥 오류가 발생했다는 내용의 토스트 메세지를 띄웁니다. 보통은 빈 칸으로 두고 넘기는 경우도 많습니다.  
<strong><ins><font color="#007AA6">중요한 건, 로그인에 성공한 경우 호출되는 onSuccess일 겁니다.</font></ins></strong> 로그인에 성공하면 로그인 API로부터 MeV2Response라는 객체가 넘어옵니다. <strong><ins><font color="#007AA6">MeV2Response는 로그인한 유저의 정보를 담고 있는 아주 중요한 객체입니다.</font></ins></strong> 보통은 이 객체에서 필요한 정보를 가져온 뒤 용도에 맞게 처리합니다.  
MeV2Response에서 가져올 수 있는 정보는 다음과 같습니다.([카카오 개발자 페이지](https://developers.kakao.com/docs/android/user-management#mev2response)에서 가져온 정보에 추가 설명을 덧붙였습니다.)

<table>
<thead>
<tr>
<th>이름</th>
<th>타입</th>
<th>설명</th>
</tr>
</thead>
<tfoot>
<tr>
<td>profileImagePath</td>
<td>String</td>
<td>640px * 640px 크기의 카카오톡 또는 카카오스토리의 프로필 이미지 URL. (경우에 따라선 480px*480px ~ 1024px*1024px인 경우도 있다.)</td>
</tr>
</tfoot>
<tbody>
<tr>
<td>id</td>
<td>Long</td>
<td>앱 유저 아이디. 로그인한 유저들을 식별하기 위한 값으로, 각 유저마다 고유한 값을 가진다.</td>
</tr>
<tr>
<td>hasSignedUp</td>
<td>OptionalBoolean</td>
<td>유저의 앱 가입 여부. 카카오의 '자동 가입'이 꺼져 있으면 작동하지 않는다. 현재 deprecated된 값.</td>
</tr>
<tr>
<td>kakaoAccount</td>
<td>UserAccount</td>
<td>유저의 카카오 계정에 관한 정보. 이메일, 나잇대, 성별 등 권한이 있어야 가져올 수 있는 정보들이 여기에 담겨 있다. (이 부분에 대한 더 자세한 설명은 카카오 로그인 API 4편을 참고하세요!)</td>
</tr>
<tr>
<td>properties</td>
<td>Map</td>
<td>유저의 앱별 정보. 닉네임, 프로필사진 주소 등을 담고 있다. 아래 값들 때문에 딱히 쓸 일 없는 값.</td>
</tr>
<tr>
<td>forPartners</td>
<td>JSONObject</td>
<td>카카오 파트너사들에게 제공되는 정보.</td>
</tr>
<tr>
<td>nickname</td>
<td>String</td>
<td>유저 닉네임.</td>
</tr>
<tr>
<td>thumbnailImagePath</td>
<td>String</td>
<td>110px * 110px 크기의 카카오톡 또는 카카오스토리의 썸네일 프로필 이미지 URL. (경우에 따라선 160px*213px 크기인 경우도 있다.)</td>
</tr>
</tbody>
</table>

 여기서는 일단 nickname과 profileImagePath만 불러와서, MainActivity로 넘겨줍니다. 닉네임을 얻어오는 함수는 getNickname(), 프로필 사진 URL을 얻어오는 함수는 getProfileImagePath()입니다. 이를 구현한 게 아래의 소스 코드죠.

```java
@Override
public void onFailure(ErrorResult errorResult) {
    int result = errorResult.getErrorCode();

    if(result == ApiErrorCode.CLIENT_ERROR_CODE) {
        Toast.makeText(getApplicationContext(), "네트워크 연결이 불안정합니다. 다시 시도해 주세요.", Toast.LENGTH_SHORT).show();
        finish();
    } else {
        Toast.makeText(getApplicationContext(),"로그인 도중 오류가 발생했습니다: "+errorResult.getErrorMessage(),Toast.LENGTH_SHORT).show();
    }
}
```

(※사실은, 아예 자체적으로 User 객체를 구현해서 그 객체에 정보를 넣어두는 방법이 가장 좋긴 합니다. 다만, 여기서는 일단 로그인 기능 자체를 구현하는 데에 중점을 두고 있으므로 그냥 intent로 넘겨주는 것까지만 하겠습니다.)  
설명이 길었죠? 이걸 다 합치면, SessionCallback이 됩니다. 로그인 버튼을 눌렀을 때 해야 할 일은 다 짠 셈이죠.  
나머지 부분은 훨씬 간단합니다. SessionCallback을 구현했으니, 이제 해야 할 일은 SessionCallback을 선언하고, 현재 세션이 콜백을 가져다가 붙이는 일 뿐입니다. <strong><ins><font color="#007AA6">onCreate 시 SessionCallback을 초기화하고, 현재 세션에 콜백을 붙입니다.</font></ins></strong> 그게 아래 소스 코드죠.

```java
public class LoginActivity extends AppCompatActivity {

    //SessionCallback 선언
    private SessionCallback sessionCallback;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        sessionCallback = new SessionCallback(); //SessionCallback 초기화
        Session.getCurrentSession().addCallback(sessionCallback); //현재 세션에 콜백 붙임
        Session.getCurrentSession().checkAndImplicitOpen(); //자동 로그인
    }
    //...
}
```

<strong><ins>여기서, checkAndImplicitOpen()이란 함수가 눈에 띄실 겁니다. 이 함수는 현재 앱에 유효한 카카오 로그인 토큰이 있다면 바로 로그인을 시켜주는 함수입니다.</ins></strong> 즉, 이전에 로그인한 기록이 있다면, 다음 번에 앱을 켰을 때 자동으로 로그인을 시켜주는 것이죠. 만약 저 함수를 주석처리하면 매 번 로그인 버튼을 눌러줘야 합니다.  
onCreate에서 현재 세션에 콜백을 붙였다면, <strong><ins><font color="#B85C00">onDestroy시에는 세션에서 콜백을 제거해야 합니다.</font></ins></strong> 네이버, 구글 등의 다른 로그인 API를 같이 사용하는 경우, 이 콜백 제거를 안 해주면 로그아웃 작업에서 문제가 생깁니다.(*실제 경험담입니다. 사실 로그아웃에서 문제가 발생했는지, 자동 로그인에서 문제가 발생했는지는 정확하게 기억이 안 납니다만, 어쨎든 꽤 크리티컬한 오류가 발생합니다.) 콜백 제거 부분이 바로 아래 코드죠.

```java
@Override
protected void onDestroy() {
    super.onDestroy();
    Session.getCurrentSession().removeCallback(sessionCallback); //현재 액티비티 제거 시 콜백도 같이 제거
}
```

 마지막으로, 카카오 로그인 액티비티에서 결과값을 가져오려면 onActivityResult 부분이 필요합니다. 아래 부분이죠. 이건 그대로 가져다가 쓰면 되서 딱히 설명할 부분도 없네요.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if(Session.getCurrentSession().handleActivityResult(requestCode, resultCode, data)) { //카카오 로그인 액티비티에서 넘어온 경우일 때 실행
        super.onActivityResult(requestCode, resultCode, data);
        return;
    }
}
```

여기까지가 LoginActivity에 대한 설명입니다. 이제, MainActivity로 넘어가서, 로그인한 정보를 써먹어 봅시다!


※ 2019.02.16 추가)  
<strong><ins>checkAndImplicitOpen() 함수는 앱이 실행될 때 가장 먼저 켜지는 Activity의 onCreate 안에 있어야 정상적으로 작동합니다.</ins></strong> 여기서는 LoginActivity가 가장 먼저 실행되기 때문에 LoginActivity의 onCreate 안에 checkAndImplicitOpen() 함수가 있었죠. 만약 앱의 시작점이 로그인 Activity가 아닌 다른 Activity(예를 들어 로딩 화면 Activity 등)라면, 로그인 Activity가 아닌 시작점인 Activity의 onCreate 안에 checkAndImplicitOpen()를 넣어주어야 합니다.
<br><br><br>

# 로그인한 유저의 이름/썸네일 주소 보여주기(MainActivity 구현)


위의 LoginActivity가 꽤 길었죠? 정작 MainActivity는 짧습니다! 그냥 intent로 받아온 정보를 TextView에 뿌려주면 끝이거든요.  
일단, 레이아웃은 아래와 같습니다. 그대로 짜실 필요는 전혀 없고, 그냥 닉네임을 표시해줄 TextView 하나, 프로필 사진 주소를 표시해줄 TextView 하나만 있으면 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="woo171tm.blog.bloglogintestkakao.MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="25dp"
        android:layout_marginTop="25dp"
        android:textSize="20sp"
        android:text="닉네임: "
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/tvNickname"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="15dp"
        android:layout_marginTop="25dp"
        android:textSize="20sp"
        android:text="..."
        app:layout_constraintStart_toEndOf="@+id/textView"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="25dp"
        android:layout_marginTop="20dp"
        android:textSize="15sp"
        android:text="프로필사진: "
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

    <TextView
        android:id="@+id/tvProfile"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:textSize="14sp"
        android:text="..."
        app:layout_constraintTop_toTopOf="@+id/textView3"
        app:layout_constraintStart_toEndOf="@+id/textView3" />

</android.support.constraint.ConstraintLayout>
```

MainActivity의 소스코드는 아래와 같습니다. (*package 이름, import 부분을 제외한 소스코드입니다.)  
간략하게 설명하자면, 닉네임을 표시하는 TextView인 tvNickname, 프로필 사진 URL을 표시하는 TextView인 tvProfile을 선언하고(10~11번째 줄), LoginActivity에서 넘어온 Intent를 받아서 String에 넣어 준 뒤(13~15번째 줄), 그 값을 tvNickname과 tvProfile에 넣습니다(17~18번째 줄).

```java
public class MainActivity extends Activity {

    String strNickname, strProfile;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TextView tvNickname = findViewById(R.id.tvNickname);
        TextView tvProfile = findViewById(R.id.tvProfile);

        Intent intent = getIntent();
        strNickname = intent.getStringExtra("name");
        strProfile = intent.getStringExtra("profile");

        tvNickname.setText(strNickname);
        tvProfile.setText(strProfile);
    }
}
```

실행 결과는 다음과 같습니다. 위의 사진과 똑같죠.

![placeholder](http://drive.google.com/uc?export=view&id=1dSER6qBxnBbFaiAOMsqOXgEFdGb1LnTY "Large example image")
첫 화면(왼쪽)에서 로그인 버튼을 누르면 오른쪽 사진처럼 나옵니다. 여기서 로그인을 하면,

![placeholder](http://drive.google.com/uc?export=view&id=1kln7bzgOk7R-WHpFyoLD-D8lQAB_IT_d "Large example image")
왼쪽처럼 제공받는 개인정보에 대한 안내창이 나오고, '동의' 버튼을 누르면 오른쪽처럼 유저 닉네임과 프로필 사진 URL이 표시됩니다.
<br><br><br>

 이번 글에서는 기본적인 카카오 로그인을 수행하고, 닉네임과 프로필 사진 주소를 가져오는 작업을 했습니다. 안드로이드 앱 개발이 익숙치 않은 분들을 기준으로 잡고, 예제 중심으로 글을 쓰다 보니 글이 좀 길어졌네요. 다음 글에서는 로그아웃 및 회원탈퇴 처리를 하는 방법에 대해 살펴보겠습니다.
