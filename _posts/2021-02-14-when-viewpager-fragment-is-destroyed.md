---
date: 2021-02-14 13:20:19
layout: post
title: "안드 꿀팁 : ViewPager의 Fragment가 onDestroyView되는 현상이 발생한다면 해결 방법은?"
subtitle:
description: "Viewpager의 Fragment 관련 팁"
image: "https://lh3.googleusercontent.com/qJeC_pp1apa-pn2RA8mJmMtU2bsqlBbAot-Dfx-tdixGCE1CYTykr8ipEFtKkiFWiE-iVG05svq-aisezpENX7f3C0jgFt2XNOD43g=w1252-rw-e365-v1"
optimized_image: "https://lh3.googleusercontent.com/qJeC_pp1apa-pn2RA8mJmMtU2bsqlBbAot-Dfx-tdixGCE1CYTykr8ipEFtKkiFWiE-iVG05svq-aisezpENX7f3C0jgFt2XNOD43g=w1252-rw-e365-v1"
category: androidkr
tags:
- android
- fragment
- viewpager
author: candykick
---

<strong><font color="#FF9300">-> 정답 : 아래와 같이 viewpager에 setOffscreenPageLimit()를 적용하면 된다!</font></strong>

```java
viewPager이름.setOffscreenPageLimit('ViewPager의 Fragment 갯수'-1);

// 예를 들어, 'viewpager'라는 이름의 ViewPager가 Fragment 3개를 가지고 있다면
viewpager.setOffscreenPageLimit(2);
```

   ViewPager는 기본적으로 이전 1개+다음 1개의 fragment의 state만 저장하고, 나머지는 onDestroyView를 호출해서 메모리에서 해제시킨다. 실제로 Fragment 3개를 가지고 있는 ViewPager로 테스트를 해봤는데, 첫 번째 Fragment가 호출될 때는 3번째 Fragment가 destroy되고, 반대로 3번째 Fragment가 호출될 때는 1번째 Fragment가 destory되었다. (이 경우 2번째 Fragment는 계속 live 상태를 유지한다. 물론 Fragment 갯수가 늘어나면 2번째 Fragment 또한 destroy 되는 경우가 생길 것이다.)

   따라서, 이를 방지하려면 ViewPager에서 상태를 저장하고 있는 fragment의 갯수를 늘려야 한다. <strong><font color="#FF9300">이를 위해 setOffscreenPageLimit 함수를 이용해서 viewPager에서 상태를 저장할 fragment의 갯수를 늘려주어야 한다.</font></strong>

p.s. 이거 찾느라고 3시간 가까이 뻘짓했다; 처음에는 Fragment가 destroy되길래 Bundle에 데이터를 저장하고, 이를 onActivityCreated() 시 불러오는 방식으로 해결하고자 했다. 그러나, 검색 도중 Fragment에서 Bundle이 저장되는 타이밍은 Activity가 onPause된 이후라는 글([링크](https://stackoverflow.com/questions/15935322/fragmentactivity-onsaveinstancestate-not-getting-called))을 보았다.

정확히는, Bundle이 저장되려면 Fragment에서 onSaveInstanceState가 실행되어야 하는데, Fragment의 onSaveInstanceState가 실행되려면 해당 Fragment를 포함하고 있는 Activity에서도 onSaveInstanceState가 실행되어야 한다. 문제는, onSaveInstanceState가 실행되는 시점은 Activity가 onPause된 이후, 즉 해당 Activity가 화면에서 사라지는 시점이라는 점이다. ViewPager의 Fragment는 Activity의 수명주기와는 상관없이 onPause와 onDestroyView가 이루어지기 때문에, 근본적으로 Bundle로는 실행할 수가 없는 것이다.