---
date: 2020-08-05 01:36:35
layout: post
title: "안드로이드로 시작하는 개발자 생활 3강 #3 레이아웃"
subtitle:
description: "안드로이드로 시작하는 개발자 생활 레이아웃"
image: "https://drive.google.com/uc?export=view&id=1ea-VkhCZW5U0VknodTxdXKwglb5J0KI7"
optimized_image: "https://drive.google.com/uc?export=view&id=1ea-VkhCZW5U0VknodTxdXKwglb5J0KI7"
category: androidstudy
tags:
- android
- huhs
- mentoring
author: candykick
---

# 3주차 레이아웃 : 영화 앱 UI 만들기(RecyclerView)
&nbsp; ★ 한 줄 요약 : 아래와 같이 cell.xml의 레이아웃을 만들어주세요. &nbsp;&nbsp;
[placeholder](https://drive.google.com/uc?export=view&id=1ea-VkhCZW5U0VknodTxdXKwglb5J0KI7 "Large example image")
&nbsp;&nbsp
## 레이아웃 조건
### ConstraintLayout(가장 바깥 부분)
* id는 container.
* 가로 길이는 match_parent, 세로 길이는 120dp.
&nbsp;
### ImageView
* 포스터 이미지 중 하나를 활용하시면 됩니다.
* id는 imgPoster.
* 가로X세로 64dpX100dp
* ImageView의 위쪽을 화면 위와 연결시키고, Top Margin은 10.
* ImageView의 왼쪽을 화면 왼쪽과 연결시키고, Left Margin은 10.
&nbsp;
### '영화 제목' TextView
* id는 tvTitle.
* TextView의 위쪽을 화면 위와 연결시키고, Top Margin은 10.
* TextView의 왼쪽을 ImageView의 오른쪽과 연결시키고, Left Margin은 15.
* 글자 크기는 24dp, 색깔은 #000000, 스타일은 굵게.
&nbsp;
### '인기도' TextView
* id는 tvPopularity.
* TextView의 위쪽을 '영화 제목' TextView와 연결시키고, Top Margin은 4.
* TextView의 왼쪽을 ImageView의 오른쪽과 연결시키고, Left Margin은 15.
* 글자 크기는 15dp, 스타일은 굵게.
&nbsp;
### '설명' TextView
* id는 tvDescription.
* TextView의 위쪽을 '인기도' TextView와 연결시키고, Top Margin은 4.
* TextView의 왼쪽을 ImageView의 오른쪽과 연결시키고, Left Margin은 15.
* 글자 크기는 13dp.
&nbsp;
### '개봉일' TextView
* id는 tvOpenDate.
* TextView의 위쪽을 '설명' TextView와 연결시키고, Top Margin은 4.
* TextView의 왼쪽을 ImageView의 오른쪽과 연결시키고, Left Margin은 15.
* 글자 크기는 14dp.
&nbsp;
