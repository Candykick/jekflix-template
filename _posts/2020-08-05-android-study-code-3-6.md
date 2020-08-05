---
date: 2020-08-04 23:45:07
layout: post
title: "RecyclerView Adapter 깡통코드"
subtitle:
description: "안드로이드로 시작하는 개발자 생활 3강 RecyclerView Adapter 코드"
image: "https://drive.google.com/uc?export=view&id=1-fOxffObIuYpQgRQma5-bDbysiYZHk5Q"
optimized_image: "https://drive.google.com/uc?export=view&id=1-fOxffObIuYpQgRQma5-bDbysiYZHk5Q"
category: androidstudy
tags:
- android
- huhs
- mentoring
author: candykick
---

# RecyclerView Adapter 깡통코드
&nbsp; ★ RecyclerView를 만드신다고요? 이 깡통코드를 응용하시면 됩니다!
&nbsp;&nbsp
## 깡통코드 쓰는 방법
1. ''로 둘러싸인 부분을 여러분 코드에 맞게 바꿔주세요. 나머지는 그대로 쓰셔도 됩니다.
2. 아래 코드를 그대로 복사-붙여넣기하지 마시고, 하나씩 입력하시면서 필요한 부분만 바꿔서 써주세요.
3. 여러분 모두의 코딩 공부를 응원합니다. ㅎㅇㅌ!
&nbsp;&nbsp
## Activity 부분
```java
class Activity이름 : AppCompatActivity() {
    //ArrayList 만들기.
    val 'ArrayList이름' : ArrayList<'Data Class 이름'> = arrayListOf(
        'Data Class 이름'('필요한 정보들 차례대로 넣기'),
        ...'필요한 횟수만큼 반복'
    )

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_movie_list)

        val adapter = 'Adapter이름'(this, 'ArrayList이름') //Adapter 선언
        'RecyclerView ID'.adapter = 'Adapter이름' //RecyclerView에 우리가 만든 MovieAdapter 셋팅

        val lm = LinearLayoutManager(this) //LinearLayoutManager 선언
        'RecyclerView ID'.layoutManager = lm //RecyclerView에 LinearLayoutManager 셋팅
    }
}
```
&nbsp;&nbsp
## Adapter 부분
RecyclerView Adapter를 넣을 코틀린 파일을 따로 만드셔야 합니다!
```java
/*
1. Movie Data Class 만들기 (O)

3. RecyclerView Adapter 만들기 (O)
 */

//data class.
data class 'Data Class 이름'(
    '필요한 변수들 선언,
    ...'필요한 갯수만큼 선언'
)

//RecyclerView Adapter
class 'Adapter이름'(val context: Context, val 'ArrayList이름': ArrayList<'Data Class 이름'>) : RecyclerView.Adapter<'Adapter이름'.Holder>() {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): Holder {
        //셀 레이아웃을 불러오는 역할.
        val view = LayoutInflater.from(context).inflate(R.layout.'셀 레이아웃 파일 이름', parent, false)
        return Holder(view)
    }

    override fun getItemCount(): Int {
        //셀 갯수를 설정하는 역할. 셀 갯수를 반환하는 함수.
        return 'ArrayList이름'.size
    }

    override fun onBindViewHolder(holder: Holder, position: Int) {
        //각 셀에 맞는 정보를 넣는 역할.
        holder.bind('ArrayList이름'[position])
    }

    //셀.
    inner class Holder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        //셀의 구성요소를 불러오는 역할.
        val 'cell을 둘러싸는 ConstraintLayout이름' = itemView.findViewById<ConstraintLayout>(R.id.'cell을 둘러싸는 ConstraintLayout ID')
        val 'View이름' = itemView.findViewById<'View타입'>(R.id.'View ID')
        ...'필요한 갯수만큼 선언

        //데이터를 셀에 넣는 역할.
        fun bind('Data Class 변수 이름': 'Data Class 이름') {//ex) movie : Movie
            '각 View에 데이터를 넣는 코드 죽 작성(아래 참고)'

            'cell을 둘러싸는 ConstraintLayout이름'.setOnClickListener { //셀을 클릭했을 때
                '셀 클릭 시 할 동작들 넣어주면 됨'
            }
        }
    }
}
```
&nbsp;&nbsp
## bind함수의 '각 View에 데이터를 넣는 코드 죽 작성'?
ImageView인 경우와, TextView인 경우가 각각 다릅니다.
아래의 각 경우에 해당하는 코드들을 구성요소에 맞게 죽 써주시면 됩니다.
```java
...
            //ImageView의 경우
            'ImageView이름'.setImageResource('적절한 이미지 ID')
            //TextView의 경우
            'TextView이름'.text = '적절한 텍스트'
...
```
