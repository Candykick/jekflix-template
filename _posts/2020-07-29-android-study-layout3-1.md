---
date: 2020-07-29 17:30:40
layout: post
title: "안드로이드로 시작하는 개발자 생활 3강 레이아웃"
subtitle:
description: "안드로이드로 시작하는 개발자 생활 레이아웃"
image: "https://drive.google.com/uc?export=view&id=1RYVPWlb1rbj4NribLxj7iO2iJQ5Hf1Ob"
optimized_image: "https://drive.google.com/uc?export=view&id=1RYVPWlb1rbj4NribLxj7iO2iJQ5Hf1Ob"
category: androidstudy
tags:
- android
- huhs
- mentoring
author: candykick
---

# 3주차 레이아웃 : 영화 앱 UI 만들기
&nbsp;★ 한 줄 요약 : 아래와 같이 main_activity.xml의 레이아웃을 만들어주세요.&nbsp;&nbsp;
&nbsp;&nbsp;
## Vertical Bias 조정하는 방법 
- 네 방향 다 아래처럼 화면과 연결시킵니다.
![placeholder](https://drive.google.com/uc?export=view&id=1_NN7POmHcTh7oSL5qtaZHWMFfCPhrJGS "Large example image")
- 오른쪽의 Attributes에서, Layout에 보면 '50'이라고 쓰여 있는 슬라이드 2개가 있습니다. 그 중 위,아래로 움직일 수 있는 슬라이드가 Vertical Bias입니다. (좌우로 움직일 수 있는 슬라이드는 Horizontal Bias)
![placeholder](https://drive.google.com/uc?export=view&id=1eZp7CX0a3lnCF0BYnUR2i3Sw__l5ahYF "Large example image")
- 위아래로 움직일 수 있는 슬라이드를 움직여서 Vertical Bias의 값을 조정합니다.
- 정확한 값으로 조정하기 어려운 경우, Vertical Bias 슬라이드를 한 번 움직인 뒤, 키보드 위/아래 키를 누르면 1 단위로 조정되니, 키보드를 이용하시면 더 정확한 조정이 가능합니다.
&nbsp;&nbsp
## 레이아웃 조건
UI는 아래와 같이 만들어주세요.
&nbsp버튼의 색깔, 버튼 글자의 색깔 바꾸는 건 모두 Common Attributes 안에 있습니다.
&nbsp버튼의 색깔을 바꾸는 옵션은 backgroundTint입니다.
![placeholder](https://drive.google.com/uc?export=view&id=1RYVPWlb1rbj4NribLxj7iO2iJQ5Hf1Ob "Large example image")
### ImageView
- movie_icon.png를 사용하시면 됩니다.
- 가로X세로 200dpX200dp
- Vertical Bias는 25
- 가로 방향 정가운데 배치.
- (네 방향 다 위의 사진처럼 화면과 연결시키면 됩니다.)
&nbsp;
### '상영 중인 영화' 버튼
- 가로는 match_constraint, 세로는 70dp
- Left Margin, Right Margin은 각각 30
- Vertical Bias는 65
- ID는 btnBoxOffice
- backgroundTint는 #65A3C9.
- textSize는 18dp, textStyle은 굵게.
- (네 방향 다 ImageView처럼 화면과 연결시키면 됩니다.)
&nbsp;
### '네이버 영화' 버튼
- 가로는 match_constraint, 세로는 70dp
- Left Margin, Right Margin은 각각 30
- Vertical Bias는 85
- ID는 btnNaverMovie
- backgroundTint는 #3F4A60.
- textSize는 18dp, textStyle은 굵게, textColor는 흰색.
- (네 방향 다 ImageView처럼 화면과 연결시키면 됩니다.)
