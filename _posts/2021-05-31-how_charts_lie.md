---
layout: post
subclass: post
title: "<숫자는 거짓말을 한다(영문 제목: How Charts Lie)>를 읽고"
date: 2021-05-31 00:00:00
tags: ["data-literacy"]
excerpt: "data-literacy"
disqus: True
---

"세상은 숫자 없이 이해할 수 없다. 그러나 세상은 숫자만으로 이해할 수도 없다."

(The world can not be understood without numbers. But the world can not be understood with numbers alone.)

통계학자이자 <팩트풀리스(Factfulness)>의 저자인 한스 로슬링의 말입니다. 수치를 통해 세상을 더욱 잘 이해할 수 있지만, 그 수치가 나온 맥락과 배경을 고려하지 않고 해석하는 것은 경계해야 한다는 말이라 생각합니다. 

숫자와 차트는 단순히 정보를 나타내는 수단이라 생각할 수 있지만, 대개는 작성자의 의도가 반영됩니다. 각국의 세계지도에서 세계의 중심이 다른 것처럼 말입니다. 

(아래 사진은 호주의 세계 지도입니다. 우리가 흔히 보는 지도와 달리 호주가 세상의 중심에 가깝게 위치하고 있습니다.)

![호주세계지도](/images/how-charts-lie/호주세계지도.jpeg)
[호주 세계지도]
이미지 출처:
[https://m.post.naver.com/viewer/postView.nhn?volumeNo=20094756&memberNo=1445907](https://m.post.naver.com/viewer/postView.nhn?volumeNo=20094756&memberNo=1445907)

<숫자는 거짓말을 한다(영문 제목: How Charts Lie)>는 이런 숫자와 차트에 쉽게 현혹될 수 있으며, 이를 주의하기 위해서 차트를 어떻게 설계하고, 해석해야 하는지를 소개하는 책입니다. 그 중 앞으로의 차트 해석에 도움이 될 수 있는 3가지 원칙을 소개합니다. 

1) 척도와 비례에 주의하기

어떤 의도를 가지고 차트를 조작할 때 가장 쉬운 방법은 척도와 비례를 조작하는 것입니다. 

![p37-12](/images/how-charts-lie/p37-12.jpeg)
[출처] '숫자는 거짓말을 한다' p37 - <그림 12> 그래프

위 그래프를 보면 부시 대통령의 감세 법안 폐지 후 세금이 무지막지하게 오를 것 같습니다. 하지만 차트의 회색 막대와 빨간 막대를 자세히 보면 그 높이와 길이가 수치에 비례하도록 표현되지 않았습니다. 차트를 해석하기 전에 차트의 척도, 범례 등을 주의 깊게 살펴보면 차트의 왜곡을 알아차리는 데 도움이 될 수 있습니다.

(차트를 설계할 때는 기준선을 영점으로 설정하고, 개체의 높이나 길이가 수치에 비례하도록 나타내야 합니다. 하지만 지구 평균기온처럼 숫자로는 2% 상승이지만 그 차이가 심각한 경우 등은 예외적으로 그 차이가 잘 드러나도록 시각화하는 것이 적절할 것입니다.)

아래는 "페이스북 통계불편러"([https://www.facebook.com/statisticallyinconvenient/](https://www.facebook.com/statisticallyinconvenient/))에서 참고한 차트입니다. 척도와 비례를 주의 깊게 보지 않으면, 선이 위쪽에 위치한 대통령일수록 직무수행 긍정률이 높은 것으로 잘못 해석할 수 있습니다.  

![p37-12](/images/how-charts-lie/president-graph.png)

2) 차트에 사용된 데이터의 신뢰도 및 편향 살펴보기

가장 먼저 차트에 사용된 데이터의 출처를 확인해야 합니다. 출처를 제시하지 않았거나 굉장히 낯설다면 일단 신뢰하지 않는 것이 좋습니다. 데이터의 출처를 신뢰할 수 있지만 선별적으로 일부 데이터만 사용하거나 데이터를 취합하여 어떤 패턴이 보이도록 하는 것도 가능합니다.

아래 그래프는 19세기 예방접종 찬반 논쟁을 불러일으켰던 수치를 참고한 가상 데이터입니다.


![p174-14](/images/how-charts-lie/p174-14.jpeg)
[출처] '숫자는 거짓말을 한다' p174 - <그림 14> 그래프

언뜻 보면 '백신 때문에 더 많은 아이들이 죽는구나'라고 생각할 수 있습니다. 하지만 위 데이터는 분모값이 누락되어 보정되지 않은 데이터입니다.

![p175-15](/images/how-charts-lie/p175-15.jpeg)
[출처 '숫자는 거짓말을 한다' p175 - <그림 15> 그래프

백신으로 인한 사망)

전체: 100만명
백신을 맞은 아동수: 100만명 X 99%(백신을 맞은 비율) = 99만명

백신을 맞고 거부반응을 일으킬 수 있는 아동수: 99만명 X 1%(거부반응을 일으킬 확률) = 9900명

거부반응을 일으킬 경우 사망할 수 있는 아동수: 9900 X 1%(거부반응을 일으킬 경우 사망할 확률) = 99명

천연두로 인한 사망)

전체: 100만명

백신을 맞지 않은 아동수: 100만명 X 1%(백신을 맞지 않은 비율) = 1만명

백신을 맞지 않은 경우 천연두에 걸릴 수 있는 아동 수: 1만명 X 2%(백신을 맞지 않은 경우 천연두에 걸릴 확률) = 200명

천연두에 걸릴 경우 사망할 수 있는 아동수: 200 X 20%(천연두에 걸릴 경우 사망할 확률) = 40명

결국 백신을 맞은 아동이 그렇지 않은 아동보다 더 많이 사망한 이유는 백신을 맞은 아이들이 그렇지 않은 아이들보다 훨씬 많았기 때문입니다. 만약 아무도 백신을 맞지 않았다면 2만명(100만명 X 2%)의 아동이 천연두에 걸렸을 것이고, 그 중 4000명(2만명 X 20%)이 사망했을 것입니다. 

차트를 확인할 때 전체 모수를 생각하는 습관이 필요하다는 생각이 듭니다. 모수에 대한 고려를 하지 않아 위 경우처럼 잘못된 판단을 할 수도 있고, 비율 데이터만을 보고 큰 수치의 차이를 공감하지 못할 수 있기 때문입니다.

![p218-1](/images/how-charts-lie/p218-1.jpeg)
[출처] '숫자는 거짓말을 한다' p218 - <그림 1> 그래프

위 차트는 1인 연간 담배 소비량과 기대수명의 관계를 나타내고 있습니다. 이 차트를 보고 담배를 피운다고 기대 수명이 줄진 않구나!라고 생각할 수 있습니다. 하지만 그렇지 않습니다. 국가의 소득수준에 따라 차트를 분리해보면 그 상관관계가 굉장히 희미해진다는 것을 알 수 있습니다. 

![p221-3](/images/how-charts-lie/p221-3.jpeg)
[출처] '숫자는 거짓말을 한다' p221 - <그림 3> 그래프

이 경우, X축(1인 연간 담배 소비량)과 Y축(기대수명)에 영향을 미치는 제3의 변수(소득수준)이 있었다는 것을 알 수 있습니다. 그러므로 이러한 차트를 봤을 때 그저 X축과 Y축의 관계로 읽어내기보다 그 관계를 왜곡하는 교란 변수(confounder)가 있는 것은 아닌지 등을 생각하며 비판적으로 수용해야겠습니다.

3) 차트에 표시된 것 이상을 넘겨짚지 않기

![p233-10](/images/how-charts-lie/p233-10.jpeg)
[출처] '숫자는 거짓말을 한다' p233 - <그림 10> 그래프

위 차트를 보면 마치 2010년 3월 건강보험 개혁 법안 통과로 취업 시장이 상당히 회복된 것으로 보입니다. 하지만 2010년 3월 건강보험 개혁 법안 통과가 된 이후의 기간에 취업자 수가 증가했을 뿐, '건강보험 개혁 법안 통과'라는 사건이 취업자 수 증가에 직접적인 영향을 끼쳤다고 말할 수는 없습니다. 2009년 2월 경기 회복 및 재투자법(American Recovery and Reinvestment Act)이 발효되었습니다. 이 법안 발효로 취업자 수가 증가했을 수도 있습니다. 즉, 취업자 수 증가에는 다른 요인의 영향이 있었을 수 있습니다. 

'상관관계는 인과관계가 아니다.'라는 말은 두 변수가 같은 방향으로 움직이는 것만으로는 인과관계를 추정할 수 없다는 것을 나타냅니다. 위 경우, 비슷한 시기에 두 사건이 발생했습니다. 하지만 비슷한 시기에 두 사건이 발생한 것만으로는 하나의 사건이 다른 사건에 영향을 주었다고 보기는 어렵습니다. 차트에 내포된 의미를 해석하는 것은 중요합니다. 하지만 차트에 표시된 것 이상을 읽어내려다 의도치 않게 곡해하지 않도록 주의해야 합니다.

"모든 차트는 현실의 단순화된 표현이며, 복잡한 현실을 숨기는 만큼 또한 많은 것을 드러낸다."는 저자의 말처럼 차트는 복잡한 세상을 단순하게 나타내줍니다. 하지만 단순함 속에 의도적으로 가려지거나 미처 표현되지 못한 부분이 있을 수 있습니다. 그렇기 때문에 숫자와 차트가 주는, 객관적이고 정확하다는 인상에 쉽게 현혹되기보다 숫자와 차트를 면밀히 검토하고 맥락을 고려하여 해석하고 의미를 도출해야겠습니다. 
