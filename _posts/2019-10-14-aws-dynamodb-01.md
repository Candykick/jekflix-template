---
date: 2019-10-14 05:17:46
layout: post
title: "AWS DynamoDB 사용하기 (1)"
subtitle: "앱 개발자의 NoSQL 서버 개발기 (1)"
description: "NoSQL DB인 DynamoDB를 사용하는 방법을 적었습니다."
image:
optimized_image:
category: serverkr
tags:
- aws
- nosql
- dynamodb
- server
author: candykick
---

★ 저는 이 글을 작성하기 전에, 이재홍님의 '아마존 웹 서비스를 다루는 기술' 원고를 죽 정독하면서 AWS의 기초를 쌓았습니다. 글 자체는 좀 옛날 글이지만, 초반 셋팅이나 설정 부분은 현재랑 큰 차이가 없기도 하고, 무엇보다 AWS에 대해 가장 심도 있게 다룬 글 중 하나라고 생각합니다. 만약 AWS를 사용하시고 싶으시다면, 꼭! 이재홍님의 글을 읽어 보시고 오실 것을 추천드립니다.  
['아마존 웹 서비스를 다루는 기술' 보러가기](http://www.pyrasis.com/private/2014/09/30/publish-the-art-of-amazon-web-services-book)


※ 참고 자료 (글 내용이 많이 수정된 관계로, 이 글에는 반영되지 않은 자료도 있습니다.)  
[AWS 자습서/NoSQL 테이블 생성 및 쿼리](https://aws.amazon.com/ko/getting-started/tutorials/create-nosql-table/)  
[AWS 자습서/Create and Manage a Nonrelational Database](https://aws.amazon.com/ko/getting-started/projects/create-manage-nonrelational-database-dynamodb/)  
[Cannot uninstall 'six'. It is a distutils installed project... #58](https://github.com/gsuitedevs/python-samples/issues/58)  
[파이썬 Flask로 간단 웹서버 구동하기](https://velog.io/@decody/파이썬-Flask로-간단-웹서버-구동하기)  
[boto3 python 2.7 ImportError: No module named boto3 USER_BASE USER_SITE site packages doesn't exist](https://stackoverflow.com/questions/42539937/boto3-python-2-7-importerror-no-module-named-boto3-user-base-user-site-site-pac)  
[AWS에 FLASK를 활용한 PYTHON 서버구축하기(가장 쉬운방법)/yonggari님 글](http://yonggari.com/set_to_python_server/)  
[웹서비스 기술 익히기 - flask 기본 사용법/잔재미코딩](https://www.fun-coding.org/flask_basic-2.html)
[Flask에서 response에 유니코드가 있을 때의 문제 해결/터프 프로그래머](https://toughrogrammer.tistory.com/222)
[Python Flask 로 간단한 REST API 작성하기/readbetweenthelines Medium](https://medium.com/@feedbotstar/python-flask-로-간단한-rest-api-작성하기-60a29a9ebd8c)



# MySQL에서 DynamoDB로 갈아탄 이유
&nbsp;&nbsp;글을 시작하기에 앞서, [이전 글](https://candykick.kr/aws-db-1/)에서 실컷 MySQL로 작업하는 이유를 적어 놓고 왜 갑자기 DynamoDB로 갈아탔는지를 좀 적어야 할 거 같습니다. 사실, 이유 자체는 한 줄로 설명 가능합니다: <strong><ins>MySQL로 짜는 것보다 DynamoDB로 짜는 게 훨씬 빠르고, 실수를 덜 할거 같아서죠.</ins></strong>  
&nbsp;&nbsp;저번 글을 적고 난 뒤, 현재 외주로 받아서 개발 중인 서비스에 어떤 테이블이 필요하고, 각 데이터가 어떻게 들어가야 할 지에 대해서 열심히 고민을 했습니다. 그런데, 상당히 많은 기능이 들어가는 앱이라서 관계형 데이터베이스로 설계하는 데에 어려움이 있었습니다. 일단 제가 서버 개발에 익숙치 않다는 게 큰 문제였습니다. 기능이 많고, 각 기능끼리 서로 연동되어 있는지라 테이블 구조를 잘못 짜면 차후 Join 작업이나 쿼리를 짜는 데에 큰 어려움을 겪을 게 뻔했죠. 또한 가만 생각해보니, 데이터를 불러오는 것 이상의 쿼리 작업이 크게 필요하지 않았고, 무엇보다도 읽는 작업이 쓰는 작업보다 훨씬 많을 것 같았습니다. 그래서, 좀 더 자세한 조사 끝에 NoSQL DB인 DynamoDB로 갈아타게 되었습니다. 다른 NoSQL DB도 있었지만, DynamoDB는 AWS에서 자체적으로 만든 NoSQL DB인지라 학습서가 잘 구비되어 있었고, 무엇보다도 파이썬으로 작업할 수 있다는 점이 매력적이어서 DynamoDB를 선택하게 되었습니다. (사실 제가 자바스크립트나 서버사이드 쪽 언어를 못하는 것도 선택에 큰 영향을 줬습니다. 그 중 하나만 할 줄 알았어도 선택의 폭이 크게 증가했을 텐데, 그 부분은 좀 아쉽네요.)


# NoSQL DB란?
&nbsp;&nbsp;일단, DynamoDB를 제대로 사용하려면 NoSQL DB에 대해 알아야 합니다. 간단히 말씀드리자면, NoSQL DB는 데이터끼리의 관계를 크게 중시하지 않는 데이터베이스라 보실 수 있습니다. 저번에 사용했던 MySQL DB랑 달리 테이블 구조에 딱딱 맞춰서 데이터를 넣어 줄 필요가 없는 데이터베이스? 라 보실 수 있죠. 안드로이드 개발자 분들이시라면 Google Firebase를 써 보셨을 겁니다. Firebase에서는 간단하게 사용할 수 있는 DB인 Cloud Firestore와 Realtime Database(실시간 데이터베이스)를 제공하는데요, 이 둘 또한 NoSQL DB의 일종입니다.


&nbsp;&nbsp;더 자세한 내용은 아래 세 글을 참고해주세요.  DB 쪽 작업이 처음이시라면, 아래 글들을 꼭! 먼저 읽어 보시는 걸 추천드립니다.
* [SQL vs NoSQL (MySQL vs. MongoDB) / 티스토리 siyoon210님](https://siyoon210.tistory.com/130) 
* [NoSQL 데이타 모델링 #1-데이타모델과, 모델링 절차 / 티스토리 조대협님](https://bcho.tistory.com/665)
* [NoSQL 데이타 모델링 #2- 데이타 모델링 패턴 / 티스토리 조대협님](https://bcho.tistory.com/666?category=431293)


# DynamoDB를 사용하기 위한 기초작업
&nbsp;&nbsp;우선, DynamoDB를 사용하려면 AWS 계정이 있어야 합니다. 그리고, AWS의 기본 개념에 대해서도 잘 알아놔야 합니다. 이 두가지는 아래 글을 참고해주세요.  
1. [AWS 기본 개념 살펴보기](http://www.pyrasis.com/book/TheArtOfAmazonWebServices/Chapter02)
2. [AWS 계정 생성하기](http://www.pyrasis.com/book/TheArtOfAmazonWebServices/Chapter03)


&nbsp;&nbsp;또한, 여기서는 파이썬 코드를 짜기 위해 아마존에서 제공하는 웹 기반 통합 개발 환경인 AWS Cloud9을 사용할 겁니다. AWS Cloud9 셋팅 과정은 다음과 같습니다.  
우선 [이 링크](https://aws.amazon.com/ko/cloud9/)를 눌러서 AWS Cloud9 페이지로 이동하고, 'AWS Cloud9 시작하기'를 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1PKxtZs_VFE6z0quSviAdF7MAC04GXR6m "Large example image")
&nbsp;&nbsp;'Create environment'를 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1rcv77aIOF5SHoGhkmPrpmhWwNpGnSA6q "Large example image")
&nbsp;&nbsp;Name에 'DynamoDB'를 입력하고 아래의 'Next step' 버튼을 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1VldIoXDrc5fOkHBz-p_JcCdMfItZOnQq "Large example image")
&nbsp;&nbsp;아래 사항 그대로 두고 'Next step'을 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1Q3vEKOkK0kx5ewSKLVAF3mHknwXuoqnV "Large example image")
&nbsp;&nbsp;'Create environment'를 누릅니다. 그리고 잠시 기다리면 아래와 같은 Welcome 페이지가 나올 겁니다.
![placeholder](http://drive.google.com/uc?export=view&id=1I8t21nH8G4IW_A53b_tQ_utbUXisSRqH "Large example image")
![placeholder](http://drive.google.com/uc?export=view&id=1DVd_j3q6MZJX_UTZEDCroWiaIq1awvvO "Large example image")
&nbsp;&nbsp;참고로 AWS Cloud9 IDE의 레이아웃은 아래와 같습니다. 사용 전에 참고 바랍니다.
![placeholder](https://d1.awsstatic.com/Getting%20Started/create-manage-non-relational-database-dynamodb/cloud%209%20consloe.34776483c7c88c238fbaa5d105666421713285fd.png "Large example image")
&nbsp;&nbsp;이제 필요한 라이브러리를 설치하겠습니다. 아래 콘솔 창에서 다음 명령어를 입력합니다. DynamoDB를 가지고 작업하기 위한 boto3 라이브러리를 설치합니다.  
```
python -m pip install --user boto3
```
![placeholder](http://drive.google.com/uc?export=view&id=1uHUoURBGAoRb5FJ3ihCBB02mqkrs9Psm "Large example image")
&nbsp;&nbsp;마지막으로, 'example'이란 폴더를 하나 만들고, 해당 폴더에서 마우스 오른쪽 버튼을 누른 뒤 'Open Terminal Here'를 누릅니다.   
![placeholder](http://drive.google.com/uc?export=view&id=1SBPEJIhzclSXobYkQQZF8VIiTXvS3J04 "Large example image")
콘솔창에서 다음 명령어를 입력합니다. 아마존에서 제공하는 DynamoDB Python용 예제 파일을 다운받습니다.
```
curl -sL https://s3.amazonaws.com/ddb-deep-dive/dynamodb.tar | tar -xv
```
![placeholder](http://drive.google.com/uc?export=view&id=1LC4tmc4QUzLC_KJHbels4BVMPYsZO57q "Large example image")


# 테이블 생성하고 데이터 넣기
&nbsp;&nbsp;실제로 써 본 경험을 바탕으로 말씀드리자면, 테이블을 생성하고 데이터를 얻어오는 과정 자체는 RDS보단 DynamoDB가 더 편했습니다. 우선 RDS랑 달리 뭔가 설치하는 과정 자체가 필요없죠. 바로 테이블을 만들 수 있게 되어 있습니다. 우선 테이블을 하나 생성해 봅시다.  
&nbsp;&nbsp;최상단의 '서비스'를 누르고, 스크롤을 조금 내려서 '데이터베이스'의 'DynamoDB'를 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=11ece9htVedaA_fE1fN7H9odSGpvY_Slr "Large example image")
&nbsp;&nbsp;리전을 잘 확인해주세요! '아시아 태평양 (서울)'로 설정해야 합니다. 만약 서울로 설정되어 있지 않다면, '아시아 태평양 (서울)'을 누릅니다. 서울로 설정되어 있어야 서울에 있는 AWS 서버에 DB가 생성됩니다.
![placeholder](http://drive.google.com/uc?export=view&id=1Lz0CmjPE-RZ347X_P2Gyp-LlH3iHsVF2 "Large example image")
&nbsp;&nbsp;가운데의 '테이블 만들기'를 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1sjTG9BklWxw-H9fkPBE9aR2pgvt4hgKF "Large example image")
&nbsp;&nbsp;테이블 이름과 기본 키를 입력합니다. 여기서 기본 키는 각 정보를 대표하는 대푯값이라고 생각하시면 되는데요, 이 값을 기준으로 테이블 전체를 정렬하거나 쿼리를 할 수 있기 때문에 중요한 값입니다.  
&nbsp;&nbsp;위에서 언급한 내용을 다시 한 번 살펴보죠. 우리는 유저 정보 테이블을 만들 거고, 각 유저 정보는 id값을 통해 검색해서 가져올 것이라고 언급했습니다. id값을 가지고 쿼리를 하는 셈이죠. <strong><ins>고로, 여기서 기본 키는 id가 되어야 합니다. id값은 문자열이니 기본 키 밑에 id만 입력하고 '문자열'은 그대로 두면 되겠죠.</ins></strong>  
&nbsp;&nbsp;입력이 끝났으면, 아래의 '생성' 버튼을 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1SnOuK74AkpqvgIqkqnfRj2UmFm3C67M8 "Large example image")
&nbsp;&nbsp;'테이블 만들기 중'이라고 나오면서, 테이블을 만드는 작업이 진행됩니다. 시간이 좀 걸리니, 잠시 기다리시면 됩니다.
![placeholder](http://drive.google.com/uc?export=view&id=1g8DqU9HOD8mlOXsL909au6VSpvxlYqRa "Large example image")
&nbsp;&nbsp;테이블이 다 만들어졌다면, 위쪽 탭 중 '항목'으로 이동합니다. 그리고 '항목 만들기' 버튼을 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1nbLwUbQUYWN_ZobKQFkOJ10IDqDRm3T0 "Large example image")
 &nbsp;&nbsp;<strong>새 컬럼을 추가할 때는 id 옆의 + 버튼을 누르고, Insert를 누른 뒤 추가할 데이터형을 누르시면 됩니다.(Append가 아닙니다! Append를 누르면 컬럼이 생기는 대신, id 안에 새 데이터가 추가됩니다.)</strong>
![placeholder](http://drive.google.com/uc?export=view&id=1AHQECp85WUw19mZakouFTuuZr688owsf "Large example image")
&nbsp;&nbsp;아래의 정보를 추가합니다. 전부 String형입니다.  
id : qwer1234  
name : Candykick  
email : woo171tm@naver.com  
moto : 항상 공부하는 개발자가 되자
![placeholder](http://drive.google.com/uc?export=view&id=1siN6t5AhNoHYmXSfN_Dcv-KRtFWBt2pg "Large example image")
&nbsp;&nbsp;데이터를 추가하면 아래와 같이 추가한 데이터가 나옵니다.
![placeholder](http://drive.google.com/uc?export=view&id=1dv-P-1aRd29fRcX0PlrJTllidNt7Zi4W "Large example image")
&nbsp;&nbsp;데이터 2개를 더 추가해 봅시다. 아래의 정보를 추가합니다. 본인 정보를 입력하셔도 좋습니다.


[1]
id : tyui6543  
name : Mameroid  
email : mameroid@haha.com  
moto : 고전게임 매니아


[2]
id : uiop0000  
name : TabsonicTop  
email : tabsonictop@naver.com  
moto : 음악게임에 빠져 지냅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1r1RjAB_ZsRI9NmPZqjJBvmyJcZtcla-a "Large example image")
&nbsp;&nbsp; 데이터 추가가 끝났습니다! 이제 입력한 데이터를 다뤄 봅시다.


# 파이썬으로 테이블 쿼리하기
&nbsp;&nbsp;테이블을 만들고, 데이터를 입력했으니, 이젠 데이터를 직접 다뤄볼 차례입니다. DB에 입력된 데이터를 가져오는 방법은 여러 가지가 있습니다만, 여기서는 가장 배우기 쉬운 파이썬을 가지고 테이블을 가져와 보겠습니다.


&nbsp;&nbsp;우선 User 테이블에 있는 모든 정보를 가져와 보겠습니다.  
AWS Cloud9으로 이동한 뒤, 위쪽의 + 버튼을 누르고 'New File'을 선택합니다.
![placeholder](http://drive.google.com/uc?export=view&id=1hgGnQ-PQFYTB09Wuw3FOG_qOqoNNmbjL "Large example image")
&nbsp;&nbsp;다음 소스 코드를 입력합니다.
```python
import boto3

dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2')
table = dynamodb.Table('User')

allData = table.scan()

print(allData['Items'])
```
![placeholder](http://drive.google.com/uc?export=view&id=1hrLCApquZA3WK_7xMKPzFYDF5QRcdMmg "Large example image")
한 줄씩 설명하자면 다음과 같습니다.  
* dynamodb : DynamoDB 객체. resource라는 함수로 가져옵니다. 뒤쪽의 region_name은 DynamoDB가 있는 리전을 가리키는데, 우리는 한국을 리전으로 잡았으므로 한국에 해당하는 값인 'ap-northeast-2'를 입력했습니다. (만약 다른 리전을 지정한 경우, [이 링크](https://docs.aws.amazon.com/ko_kr/general/latest/gr/rande.html)의 'Amazon API Gateway'에서 각 리전에 해당하는 문자열을 보실 수 있습니다.)
* table : DynamoDB 테이블. <strong><ins><font color="#007AA6">db객체.Table('테이블명')으로 테이블명에 해당하는 테이블을 가져옵니다.</font></ins></strong>여기서는 'User'라는 이름의 테이블을 가져옵니다.
* allData : 'User'테이블의 모든 데이터. json 형태입니다. <strong><ins><font color="#007AA6">테이블객체.scan() 함수를 통해 모든 데이터를 가져올 수 있습니다.</font></ins></strong>
* 이후 print를 통해 allData의 내용을 콘솔창에 출력합니다.


&nbsp;&nbsp;Ctrl+S(맥은 Command+S)키를 누르고 'scan_data.py' 저장합니다. 끝에 .py 확장자를 붙여야 파이썬 파일로 저장됩니다.
![placeholder](http://drive.google.com/uc?export=view&id=1lYzsQ3CBk2OZyBBAUMu-oSaiQ-p1i8uH "Large example image")
&nbsp;&nbsp;해당 파일을 실행시켜봅시다. scan_data.py 파일이 있는 폴더(여기서는 DynamoDB)를 선택하고, 마우스 오른쪽 버튼을 누른 뒤 'Open Terminal Here'를 누릅니다.
![placeholder](http://drive.google.com/uc?export=view&id=1YtdQWDa6NcVHqzDBtazvKqaEqS5zRP5L "Large example image")
&nbsp;&nbsp;다음 명령어를 콘솔창에 입력해 scan_data.py를 실행시킵니다.

    python scan_data.py
![placeholder](http://drive.google.com/uc?export=view&id=17yTh_xOk34N7p9XotPCdYfAdGDtqqWKa "Large example image")
&nbsp;&nbsp;실행시키면 다음과 같이 모든 정보를 불러옵니다.
![placeholder](http://drive.google.com/uc?export=view&id=1RSmuQunBDdsGvhTmP08qogDr08sSxeDl "Large example image")


&nbsp;&nbsp;이번에는 id값을 가지고 쿼리를 해보겠습니다.
새 파일을 하나 생성하고, 다음 소스 코드를 입력합니다. 파일 이름은 'query_data.py'로 저장합니다.
```python
import boto3

dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2')
table = dynamodb.Table('User')

qdata = table.get_item(Key={"id": "qwer1234"})

print(qdata['Item'])
```
![placeholder](http://drive.google.com/uc?export=view&id=10rhD1yvx830QXRKKnDrzu_bhsMPhH2S6 "Large example image")
&nbsp;&nbsp;DynamoDB 밑 테이블을 선언하는 부분은 그대로이니, 쿼리 부분만 설명하겠습니다.  
<strong><ins><font color="#007AA6">테이블객체.get_item(Key={"컬럼": "검색하려는 값"}) 함수를 통해 특정값을 가진 데이터를 가져올 수 있습니다. 여기서는 컬럼에 id를, 검색하려는 값에 qwer1234를 넣어서 id가 qwer1234인 데이터를 가져왔습니다.</font></ins></strong> 가져온 데이터는 json 형태입니다.  
실행해보면 아래와 같이 나옵니다.
![placeholder](http://drive.google.com/uc?export=view&id=1B2ZYktAB9_V13WPkkkdveBcnmVv1vzKl "Large example image")


&nbsp;&nbsp;특정 값을 찾는 것 말고도, 특정 범위의 값을 찾아내거나, 2가지 이상의 조건을 걸 수도 있습니다. 쿼리에 대한 더 자세한 내용은 추후에 작성하겠습니다!  
일단 파이썬으로 쿼리를 해 오는 방법을 알았으니, 이걸 실제 웹페이지에 출력시키는 작업을 해 봅시다. 근데 그 작업을 하려면, 웹페이지를 올릴 서버가 있어야 되고, 웹페이지도 만들어 놔야겠죠? 그래서 이제부턴 EC2 서버를 만들고, Flask로 웹페이지를 만들어서 서버에 올려보겠습니다. 


# Flask 웹페이지 만들고 EC2 서버에 올리기
&nbsp;&nbsp;위의 소스 코드를 그대로 쓸 수 있다면 좋겠지만, 현실은 만만치 않죠? 안타깝게도, 위의 코드를 그대로 쓸 수는 없습니다. 왜냐하면, 안드로이드 앱이나 iOS 앱에서 파이썬 코드가 내놓는 결과물을 가져올 수 없기 때문입니다. 위의 결과물은 어디까지나 콘솔창에서 출력을 하고 있으니까요.  
그래서, 위의 파이썬 코드에 작업을 좀 더 해야 합니다. <strong>쿼리한 내용을 웹으로 볼 수 있게 만드는 게 그 작업이죠.</strong>이 작업을 하고 나면, 우리는 앱에서 웹페이지에 get이나 post로 필요한 값을 넘겨주기만 하면 됩니다. 그러면 웹페이지에서 쿼리를 수행한 후 결과값을 앱으로 돌려줍니다.  
그러면 이젠 Html까지 다뤄야 할까요? 그럴 필요 없습니다! 파이썬 라이브러리인 Flask를 이용하면 간단한 웹페이지를 만들 수 있습니다. 어차피 레이아웃은 필요없고, 결과값을 json으로 보여주기만 하면 되기 때문에, 우리는 파이썬만 가지고 작업할 겁니다.


(* 여담으로, 원래는 Cloud9 위에서 Flask를 구동하는 방법을 찾아봤었습니다. Cloud9 위에서 구동만 되면 테스트하는 게 훨씬 수월하기 때문입니다. 그런데 이것저것 다양한 수를 써보고, 여러 사이트를 뒤져가며 2시간 넘게 삽질을 해봤는데도 Cloud9 위에선 Flask가 정상적으로 작동하질 않더라고요. 그래서 여기서는 좀 번거롭지만 EC2 서버에 소스코드를 올려서 테스트하는 방식을 썼습니다. 어차피 실제 서비스를 하려면 EC2 서버에 올려야 되기도 하고, 쿼리 내용에 Flask만 조금 추가하면 최종 코드가 완성되기 때문에 엄청 번거롭진 않을 거라고 생각합니다. <ins>혹시라도, Cloud9 위에서 Flask를 구동하는 방법을 아시는 분은 댓글 남겨주시면 감사하겠습니다!</ins>)


&nbsp;&nbsp;우선 EC2 서버를 만들고 관련 설정을 합시다.  
AWS 콘솔 페이지에 가서, '서비스' > '컴퓨팅' 바로 밑의 'EC2'를 선택합니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;'인스턴스 시작' 버튼을 누릅니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;AMI를 선택하라는 창이 나옵니다. AMI는 운영체제를 포함한 EC2 이미지입니다. 우분투 기반 서버를 설치해야 하므로 스크롤을 조금 내려서 <strong>'Ubuntu Server'</strong>로 시작하는 걸 선택합니다. <strong><ins>'프리 티어 사용 가능'이라고 표시된 걸 선택해야 합니다! 이게 없는 걸 선택하면 사용료가 과금됩니다.</strong></ins>
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;인스턴스 유형을 선택하는 창이 나옵니다. 다른 걸 선택하면 사용료가 나가므로, 그대로 두고 '검토 및 시작'을 누릅니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;'시작하기'를 누릅니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;EC2 서버 인스턴스를 볼 수 있는 창이 나옵니다. 잠시 기다리면 상태가 running으로 바뀌면서 사용 가능한 상태가 됩니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;EC2 서버 인스턴스를 볼 수 있는 창이 나옵니다. 잠시 기다리면 상태가 running으로 바뀌면서 사용 가능한 상태가 됩니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;'설명' 탭을 아래로 좀 내려서 '보안 그룹'을 찾습니다. '보안 그룹'옆에 보안 그룹명이 적혀 있을 겁니다. 따로 설정하지 않았다면 'launch-wizard-숫자'로 되어 있을 텐데요, 이 글씨를 클릭합니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")


&nbsp;&nbsp;EC2 서버를 만들었으면, 서버에 접속해야 합니다. 서버 접속 방법은 아래 두 링크를 참고 바랍니다.  
* [윈도우에서 EC2 서버에 접속하기](http://www.pyrasis.com/book/TheArtOfAmazonWebServices/Chapter04/04)
* [맥, 리눅스에서 EC2 서버에 접속하기](http://www.pyrasis.com/book/TheArtOfAmazonWebServices/Chapter04/04/02)


&nbsp;&nbsp;서버에 접속했으면 아래 순서대로 설치를 해봅시다.
1. 우분투 서버를 업그레이드 및 업데이트합니다.  
```
sudo apt-get update
sudo apt-get upgrade
```
2. 파이썬이 설치되어 있지 않다면 파이썬을 설치합니다.  
```
sudo apt-get install python3.5
```
3. 파이썬 라이브러리 설치를 도와주는 pip를 설치합니다.  
```
sudo apt-get install python3-pip
```
4. 파이썬 서버인 Flask, DynamoDB에서 결과값을 얻어오기 위한 boto3를 설치합니다.  
```
sudo pip3 install flask
sudo pip3 install boto3
```


&nbsp;&nbsp;설치가 끝났습니다! 일단은 Hello, World 먼저 작성해 봅시다.  
콘솔창에 다음과 같이 입력해서 run.py라는 파일을 생성하고 nano 편집기를 엽니다.  
```
nano run.py
```
아래 코드를 입력합니다.  
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run(host = '0.0.0.0', port = 5000, debug = True)
```
&nbsp;&nbsp;다른 부분은 중요하지 않고, 두 부분만 살펴보겠습니다.  
* <strong>@app.route("/") : 생성할 페이지의 경로입니다.</strong> 여기서는 하위 페이지 없이 서버 주소로 접근 시 바로 Hello World 페이지를 띄우도록 했습니다. 만약 <strong>@app.route("/json_test")</strong>와 같이 입력한다면 json_test라는 이름의 페이지가 하나 생기고, 해당 페이지는 "서버 주소/json_test"로 접근할 수 있게 됩니다.
* <strong>return "Hello World!" : 표시할 문자열을 반환합니다. 문자열 말고 json 형식이나 기타 몇몇 다른 형식도 반환 가능합니다.</strong> return 다음 부분에 DynamoDB에서 받아온 json을 넣어주면 해당 json을 보여주겠죠?


&nbsp;&nbsp;페이지가 실제로 실행되는지 살펴봅시다.  
우선 입력한 소스 코드를 Ctrl+O키를 눌러 저장하고, Ctrl+X로 nano에서 빠져나옵니다.  
그 후, 콘솔창에 아래 코드를 입력해 파이썬 코드를 빌드합니다.
```
python3 run.py
```
&nbsp;&nbsp;그리고 AWS 콘솔 페이지에 접속해서, EC2 인스턴스 페이지로 들어갑니다. 현재 작동중인 EC2 서버를 클릭하고 아래 '설명' 부분을 보면 '퍼블릭 DNS(IPv4)'가 보일 겁니다. 여기 표시된 주소가 서버 주소입니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
이 주소 뒤에 포트 번호 5000을 붙여 주면 됩니다. 여기서는 <strong><ins><font color="#007AA6">"http://000-00-000-00-000.ap-northeast-2.compute.amazonaws.com:5000"가 되겠죠.</font></ins></strong> 접속해보면 아래와 같이 나오는 걸 보실 수 있습니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")


&nbsp;&nbsp;이제, 이 페이지에 아까 우리가 짠 DynamoDB 쿼리 소스를 넣으면 됩니다. 그 작업을 해 봅시다!


# Flask 웹페이지에 DynamoDB 쿼리 결과 출력하기
&nbsp;&nbsp;일단, User테이블 안에 있는 모든 데이터를 가져오는 소스코드를 Flask 웹페이지에 추가해 봅시다.
잘 버무려보면(?) 아래와 같은 모양새가 될 겁니다.  
```python
from flask import Flask
import boto3

app = Flask(__name__)

@app.route('/')
def hello():
    dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2')
    table = dynamodb.Table('User')
    allData = table.scan()
    return allData['Items']

if __name__ == '__main__':
    app.run(host = '0.0.0.0', port = 5000, debug = True)
```
&nbsp;&nbsp;뭐, 별 건 없습니다. 위에서 짰던 scan 소스코드를 hello() 구문 안에 넣어서, User테이블 안의 모든 데이터를 json 형태로 저장한 allData['Items']를 반환하는 거죠. 아까 분명히 json형태도 return 가능하다고 했으니까 문제는 없어 보입니다.  
하지만, 이런! 이렇게 해 놓고 실행해보면 오류가 주루룩 나옵니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;타입 에러라는군요. 반환된 형태가 리스트라서 안 된다는 듯 하네요. 뭔가 후처리가 더 필요한 모양입니다!  
Flask에서는 json 형태를 리턴하기 위한 함수가 따로 존재합니다. 바로 <strong><font color="#007AA6">jsonify</font></strong> 함수죠. <strong><font color="#007AA6">jsonify(json객체)</font></strong>의 형태로 사용하면 됩니다. 이 함수를 추가해서 다시 짜 봅시다.
```python
from flask import Flask, jsonify
import boto3

app = Flask(__name__)

@app.route('/')
def hello():
    dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2')
    table = dynamodb.Table('User')
    allData = table.scan()
    return jsonify(allData['Items'])

if __name__ == '__main__':
    app.run(host = '0.0.0.0', port = 5000, debug = True)
```
&nbsp;&nbsp;<strong>주의 : 최상단 import 부분에 jsonify를 추가해줘야 합니다!</strong> 이렇게 짜고 다시 실행해 봅시다. 
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;음, 일단 나오긴 하는데, 제대로 된 결과값은 아닙니다. 한글 부분이 다 깨져서 나오네요.  
한글 부분 인코딩을 하려면 소스 코드에 뭔가를 더 추가해야 할 거 같습니다. 한글 인코딩 처리를 추가해서 다시 짜 보면 아래와 같습니다. (이 소스 코드는 제가 자세히 아는 게 아니라서 내용을 짧게 적었습니다. 코드에 대한 설명은 이 링크를 참고하세요: [Flask에서 response에 유니코드가 있을 때의 문제 해결/터프 프로그래머](https://toughrogrammer.tistory.com/222))  
```python
from flask import Flask, Response
import boto3
from functools import wraps
import json

app = Flask(__name__)

def as_json(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        res = f(*args, **kwargs)
        res = json.dumps(res, ensure_ascii=False).encode('utf8')
        return Response(res, content_type='application/json; charset=utf-8')
    return decorated_function

@app.route('/')
@as_json
def hello():
    dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2')
    table = dynamodb.Table('User')
    allData = table.scan()
    return allData['Items']

if __name__ == '__main__':
    app.run(host = '0.0.0.0', port = 5000, debug = True)
```
&nbsp;&nbsp;위쪽의 <strong><font color="#007AA6">as_json(f): </font></strong>부분은 그대로 쓰시면 됩니다. 넘어오는 json 값을 utf-8로 인코딩하는 함수라 생각하시면 됩니다. 이 데코레이터를 아래의 hello() 부분 위에 <strong><font color="#007AA6">@as_json</font></strong>과 같이 적용해주면 됩니다.    
그리고, <strong><ins>이 과정에서 아이러니하게도 jsonify가 빠졌죠.</ins></strong> 한글로 인코딩해주는 as_json 함수가 jsonify가 할 일까지 다 하기 때문에 jsonify가 필요가 없습니다. 따라서, <font color="#007433"><strong>json 결과물에 한글이 포함되어 있다면 as_json을 포함한 코드로 작업하시고, 영어나 숫자만 있다면 jsonify를 쓰시면 됩니다.</strong></font>   
실행해보면 아래와 같이 나옵니다. 한글도 정상적으로 출력되죠.
![placeholder](aaaaaaaaaaaaaa "Large example image")


&nbsp;&nbsp;이젠 페이지를 2개를 만들어 봅시다. 다음 2개의 페이지를 만들어 보겠습니다.  
* allData : 모든 데이터를 보여주는 페이지. (scan() 함수로 쿼리.)
* queryById : id값으로 쿼리한 결과를 보여주는 페이지. (get_item() 함수로 쿼리.)
<strong>아까 만든 run.py에 id값으로 쿼리하는 소스 코드를 추가해주고, @app.route('/queryById')를 지정해주면 될 겁니다. 결과값에는 한글도 포함되어 있으니 @as_json도 붙여줘야겠죠. 그리고 기존의 모든 데이터를 보여주는 페이지의 주소도 바꿔야 하니, 해당 부분은 @app.route('/allData')로 고치면 될 겁니다. 정리해보면 아래와 같죠.</strong>  
```python
from flask import Flask, Response
import boto3
from functools import wraps
import json

app = Flask(__name__)

def as_json(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        res = f(*args, **kwargs)
        res = json.dumps(res, ensure_ascii=False).encode('utf8')
        return Response(res, content_type='application/json; charset=utf-8')
    return decorated_function

@app.route('/allData')
@as_json
def allData():
    dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2')
    table = dynamodb.Table('User')
    allData = table.scan()
    return allData['Items']
    
@app.route('/queryById')
@as_json
def queryById():
    dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2')
    table = dynamodb.Table('User')
    qdata = table.get_item(Key={"id": "qwer1234"})
    return qdata['Item']

if __name__ == '__main__':
    app.run(host = '0.0.0.0', port = 5000, debug = True)
```
&nbsp;&nbsp;실행해보면 아래와 같습니다. 일단 원래 페이지가 있었던 주소인 "서버 주소:5000"(여기서는 "http://000-00-000-00-000.ap-northeast-2.compute.amazonaws.com:5000")로 접속하면 Not Found 페이지가 나오죠. index 페이지를 없앤 셈이니, 당연한 겁니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;대신 페이지 주소를 맞게 지정해주면 결과값이 잘 출력됩니다. 모든 데이터를 출력하는 allData 페이지("서버 주소:5000/allData". 여기서는 "http://000-00-000-00-000.ap-northeast-2.compute.amazonaws.com:5000/allData")에 접속하면 아래와 같이 나옵니다.
![placeholder](aaaaaaaaaaaaaa "Large example image")
&nbsp;&nbsp;id값이 qwer1234인 데이터를 출력하는 queryById 페이지("서버 주소:5000/queryById". 여기서는 "http://000-00-000-00-000.ap-northeast-2.compute.amazonaws.com:5000/queryById")에 접속하면 아래와 같이 나오죠.
![placeholder](aaaaaaaaaaaaaa "Large example image")


# Flask 페이지로 GET/POST 요청 보내기
&nbsp;&nbsp;이제, 오늘의 마지막 작업을 해 봅시다. allData 페이지는 그대로 써도 별 문제가 없습니다. 그러나, queryById 페이지는 문제가 있죠. 지금은 id값이 qwer1234인 데이터만 돌려줍니다. 하지만, 특정 id값만 검색해서 보여주는 건 별 쓸모가 없습니다. 페이지를 실행하기 전에 id값을 받아서, 그 id값으로 검색한 결과값을 돌려줘야 의미가 있겠죠.  
GET이나 POST 방식을 활용해서 Flask로 만든 웹페이지에 id값을 보내면 됩니다. 만약 GET, POST 방식에 대해 처음 들어보신다면,  [Flask로 GET, POST 요청 보내기 (1)/강민우님 Medium](https://medium.com/@mystar09070907/웹-페이지-client-에서-정보-보내기-bf3aff952d3d)을 참고해주세요.  


&nbsp;&nbsp;일단 flask-restful 라이브러리를 설치합니다.  
```
sudo pip3 install flask-restful
```
그리고 run.py를 엽니다.  
```
nano run.py
```
