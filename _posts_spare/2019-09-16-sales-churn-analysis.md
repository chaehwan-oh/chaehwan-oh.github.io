---
layout: post
subclass: post
title: "예제를 활용한 데이터 분석 - 매출 분석/이탈 분석"
date: 2019-09-09
tags: [data-analysis, python, pandas]
excerpt: "pandas를 활용한 간단한 데이터 분석 내용을 정리해봤습니다."
disqus: True
---

# Intro

파이썬을 굉장히 좋아하고 업무에서도 사용하고 있지만 막상 표 형태의 정형 데이터를 다룰 때 생각도 못한 곳에서 애를 먹는 경우가 제법 있었습니다.
업무에서의 생산성 향상을 위해 간단한 예제 케이스를 분석해보도록 하겠습니다. 실생활 데이터가 아니지만 연습을 위해서는 충분할 것이라고 생각합니다.
프로그래밍 경험이 없는 분도 간단히 따라해보시면 좋은 경험이 될 것이라고 생각합니다.

진행 전 준비해야 할 것이 있습니다.

1. 파이썬 설치

출처 - [생활코딩 파이썬 설치 방법](https://www.opentutorials.org/course/1750/9610)

2. 개발도구 설치

[PYCHARM 설치](https://www.jetbrains.com/pycharm/)
(free community 버전을 설치하시면 됩니다.)

3. 데이터 다운로드받기

[Git 저장소에서 분석 데이터와 코드 내려받기](https://github.com/chaehwan-oh/analysis-case-study)

git이 익숙하는 분들은 git을 사용해 저장소를 내려받으시면 되고 git이 익숙하지 않은 분들은 위 링크에서 Clone or download > Download ZIP 버튼을
눌러 zip 파일을 다운로드받으시면 됩니다.

이제 시작해보도록 하겠습니다.

# 퍼즐 게임 애플리케이션의 매출 하락 분석해보기

퍼즐 게임 애플리케이션을 운영하고 있던 회사의 매출은 순조롭게 늘어나고 있었습니다. 하지만 이달 들어 갑자기 매출이 줄어들었습니다.
실무진의 논의 결과, 아래와 같은 가설을 세울 수 있었습니다.

"예산 문제로 지난 달만큼 광고를 하지 못했다. 광고가 줄면서 신규 유저를 충분히 확보하지 못했고 이에 따라 매출이 하락했다."

그러므로 광고의 효과에 대한 분석 이전에 신규 유저의 감소가 매출 감소에 영향을 미쳤다는 것을 파악하려고 합니다. 이를 위해 필요한
데이터는 아래와 같습니다.

```
DAU(Daily Active User): 하루에 한 번 이상 게임을 이용한 유저 데이터
DPU(Daily Payment User): 하루에 1원 이상 지불한 유저 데이터
Install: 유저별로 게임을 이용하기 시작한 날짜가 기록된 데이터
```

- DAU

| 데이터 내용 |  데이터 타입   |     변수명 |
| ----------- | :------------: | ---------: |
| 로그인한 날 | string(문자열) | login_date |
| 앱 이름     | string(문자열) |   app_name |
| 유저 ID     |   int(수치)    |    user_id |

- DPU

| 데이터 내용 |  데이터 타입   |     변수명 |
| ----------- | :------------: | ---------: |
| 과금일      | string(문자열) | login_date |
| 앱 이름     | string(문자열) |   app_name |
| 유저 ID     |   int(수치)    |    user_id |
| 과금액      |   int(수치)    |    payment |

- Install

| 데이터 내용    |  데이터 타입   |       변수명 |
| -------------- | :------------: | -----------: |
| 이용 시작한 날 | string(문자열) | install_date |
| 앱 이름        | string(문자열) |     app_name |
| 유저 ID        |   int(수치)    |      user_id |

위 데이터는 csv파일로 확보해둔 상태라고 가정하겠습니다. analysis-case-study 디렉토리의 data 디렉토리에 위 csv 파일들을 저장해두었습니다.
(csv 파일을 comma seperated values라는 뜻으로 각 행과 열의 값들을 쉼표로 구분한 파일이라고 생각하시면 됩니다.)

먼저 위 파일의 값들을 읽어보겠습니다. 이를 위해서는 python 언어에서 데이터 분석을 위해 주로 사용하는 pandas 라이브러리를 먼저 다운로드받아야 합니다.

```
pip3 install pandas
```

위 명령어를 pycharm 개발환경의 하단에 있는 terminal 버튼을 클릭한 다음, 입력해 실행하시면 됩니다. 그런 다음 아래의 명령어로
Daily Active User, Daily Payment User, Install 데이터를 읽어옵니다.

## 데이터 불러오기

```python
import pandas as pd


daily_active_user = pd.read_csv("../data/dau.csv")
install = pd.read_csv("../data/install.csv")
daily_payment_user = pd.read_csv("../data/dpu.csv")

print(daily_active_user.head())
print(install.head())
print(daily_payment_user.head())
```

위 명령어를 통해 필요한 데이터를 읽고 온 다음 표 형태의 데이터의 첫 몇 행을 확인할 수 있습니다. 기본으로 5행을 확인할 수 있습니다.
파이참에서 디버거를 활용하실 수 있는 분들은 디버거로 테이블의 형태를 더욱 보기 좋게 확인하실 수 있습니다. 아래의 링크를 참고하시길 바랍니다.

[파이참에서 데이터 프레임 확인하기](https://www.jetbrains.com/help/pycharm/viewing-as-array.html)

표를 읽어들인 다음에는 형식을 확인한 다음 각 표가 나타내는 정보를 파악할 수 있습니다.
예를 들어 daily_active_user 테이블의 아래 행의 정보는 '2019년 6월 1일 퍼즐컬렉션 앱에 116번 사용자가 로그인했다.'라는 것을 알 수 있습니다.

| 로그인한 날    |  앱 이름   | 유저 ID |
| -------------- | :--------: | ------: |
| 2019년 6월 1일 | 퍼즐컬렉션 |     116 |

## 데이터 결합하기

이번 분석에서 필요한 것은 '매출 감소가 신규 유저의 영향인지 그렇지 않은지' 파악하는 것입니다. 이를 위해 불러온 3개의 테이블 데이터를 병합해 하나의 테이블로
만들 것입니다.

```python
import pandas as pd


daily_active_user = pd.read_csv("../data/dau.csv")
install = pd.read_csv("../data/install.csv")
daily_payment_user = pd.read_csv("../data/dpu.csv")

user_install_log = pd.merge(daily_active_user, install, on=['user_id', 'app_name'])
user_log = pd.merge(user_install_log, daily_payment_user, on=["login_date", "app_name", "user_id"])
print(user_log.head())
```

각 테이블을 pandas 라이브러리의 merge 함수를 사용해, on의 매개변수로 넘겨주는 값들을 기준으로 테이블을 합쳐 하나의 테이블을 생성합니다.
생성된 테이블은 아래와 같은 형태가 됩니다.

![merged_table](/images/data-analysis/user_log.png)

## 월별로 집계하기

```python
import pandas as pd

daily_active_user = pd.read_csv("../data/dau.csv")
install = pd.read_csv("../data/install.csv")
daily_payment_user = pd.read_csv("../data/dpu.csv")

user_install_log = pd.merge(daily_active_user, install, on=['user_id', 'app_name'])
user_log = pd.merge(user_install_log, daily_payment_user, on=["login_date", "app_name", "user_id"])

user_log['login_month'] = user_log['login_date'].map(lambda x: x[:7])
user_log['install_month'] = user_log['install_date'].map(lambda x: x[:7])
monthly_active_user = user_log.groupby(['login_month', 'user_id', 'install_month'], as_index=False).sum()
print(monthly_active_user)
```

테이블을 합쳐 필요한 데이터를 하나의 테이블에 확보했다면 그 다음으로 필요한 것은 월별로 결제를 집계하는 것입니다. 그 이유는 신규 유저와 기존 유저간의
매출을 비교하여야 하기 때문입니다. '2019-06-03'라는 전체 날짜 데이터에서 '2019-06'라는 부분 문자열을 추출한 다음 월을 나타내는 부분 문자열을
기준으로 집계할 수 있습니다. groupby 함수 적용시 as_index=False로 값을 넘겨주는 이유는 기존의 column을 유지하기 위함입니다.
그렇지 않으면 column 값들이 모두 index 값으로 변경되기 때문입니다.

## 신규/기존 유저 구분하기

```python
monthly_active_user['user_type'] = monthly_active_user.apply(
    lambda x: 'new' if x['install_month'] == x['login_month'] else 'existing', axis=1)
print(monthly_active_user)
```

월별로 집계한 다음 필요한 작업은 이용 시작한 달과 로그인한 달을 비교하여 같은 달이면 '신규', 다르면 '기존' 유저로 분류해주는 것입니다.
사용자의 유형 값을 column으로 넣어줌으로써 더욱 쉽게 나누어 집계하고 시각화하는 것이 가능합니다.

## 데이터 시각화하기

```python
sales_summary = monthly_active_user.groupby(['login_month'], as_index=False)['payment'].sum()
print(sales_summary)
sales_summary.plot(kind='bar', x='login_month', y='payment')
plt.title('Sales comparison')
plt.show()

new_user = monthly_active_user[monthly_active_user['user_type'] == 'new']
new_user_sales_summary = new_user.groupby(['login_month'], as_index=False)['payment'].sum()
new_user_sales_summary.plot(kind='bar', x='login_month', y='payment')
plt.title('New user sales comparison')
plt.show()
```

그런 다음 마지막은 지난 달과 이달의 매출을 비교하고 신규 유저의 지난 달과 이달의 매출을 비교하는 것입니다.
시각화한 결과는 다음과 같습니다.

[지난달과 이달의 매출 비교]

![sales_comparison](/images/data-analysis/sales_comparison.png)

[지난달과 이달의 신규 사용자 매출 비교]

![new_users_sales_comparison](/images/data-analysis/new_users_sales_comparison.png)

이렇게 실제 데이터는 아니지만 지난 달과 이달의 신규 사용자 매출에 차이가 있음을 파악할 수 있었습니다. 대신 이것이 광고의 영향력인지는 분석해보아야 할
문제입니다. 이 분석을 통해서는 매출의 감소가 신규 유저를 통한 매출 감소로 인한 것이었는지를 파악할 수 있었습니다.

전체 코드는 아래의 링크를 통해 확인할 수 있습니다.

[sales_analysis](https://github.com/chaehwan-oh/analysis-case-study/blob/master/src/sales_analysis.py)

# 퍼즐 게임 애플리케이션의 이탈 분석해보기

다음 분석은 고객의 이탈을 분석해보는 것입니다. '지난달과 비교해서 유저의 수가 줄었다'는 현상이 발견되었습니다. 광고와 이벤트에 문제가 있는지 파악해본
결과 크게 달라진 부분이 없었습니다. 그러므로 성별 혹은 연령 등을 기준으로 어떤 유저층에서 이탈한 유저가 많았는지 파악해보기로 했습니다. 아래의 데이터를
활용해 분석을 진행했습니다.

```
DAU(Daily Active User): 하루에 한 번 이상 게임을 이용한 유저 데이터
user_info: 유저의 속성정보 데이터
```

- DAU

| 데이터 내용 |  데이터 타입   |     변수명 |
| ----------- | :------------: | ---------: |
| 로그인한 날 | string(문자열) | login_date |
| 앱 이름     | string(문자열) |   app_name |
| 유저 ID     |   int(수치)    |    user_id |

- user_info

| 데이터 내용               |  데이터 타입   |       변수명 |
| ------------------------- | :------------: | -----------: |
| 이용시작일                | string(문자열) | install_date |
| 앱 이름                   | string(문자열) |     app_name |
| 유저 ID                   |   int(수치)    |      user_id |
| 성별(여성,남성)           | string(문자열) |       Gender |
| 연령대                    |   int(수치)    |   Generation |
| 단말기 종류(iOS, Android) | string(문자열) |  device_type |

## 데이터 불러오기

가장 먼저 진행해야 하는 것은 필요한 데이터를 불러오는 것입니다. 매출 분석과 유사하게 아래와 같이 데이터를 불러올 수 있습니다.

```python
import pandas as pd

daily_active_user = pd.read_csv('../data/dau2.csv')
user_info = pd.read_csv('../data/user_info.csv')

print(daily_active_user.head())
print(user_info.head())
```

데이터를 불러온 다음 head 함수를 통해 데이터의 형태를 파악할 수 있습니다.

## 데이터 결합하기

```python
daily_active_user_info = pd.merge(daily_active_user, user_info, on=['user_id', 'app_name'])
print(daily_active_user_info.head())
```

분석을 통해 알고 싶은 것은 어떤 속성을 가진 사용자가 게임을 이용하는가 그렇지 않은가입니다. 그러므로 이를 파악하기 위해 속성정보를 담고 있는 user_info
테이블과 로그인 정보를 담고 있는 daily_active_user 테이블을 결합해야 합니다.

## 크로스 집계하기

결합한 다음 필요한 것은 크로스 집계를 통해 특정 유저층의 변화가 기간에 따라 어떻게 변화했는지 살펴보는 것입니다. 아래의 코드를 통해 크로스 집계를 만들고
기간에 따른 변화를 살펴볼 수 있습니다.

```python
daily_active_user_info['login_month'] = daily_active_user_info['login_date'].map(lambda x: x[:7])
print(daily_active_user_info.head())

cross_table_by_gender = pd.crosstab(
  daily_active_user_info['login_month'], daily_active_user_info['gender'])

cross_table_by_generation = pd.crosstab(
  daily_active_user_info['login_month'], daily_active_user_info['generation'])

cross_table_by_gender_with_generation = pd.crosstab(
  daily_active_user_info['login_month'], [daily_active_user_info['gender'], daily_active_user_info['generation']])

cross_table_by_device = pd.crosstab(daily_active_user_info['login_month'], daily_active_user_info['device_type'])

print(cross_table_by_gender)
print(cross_table_by_generation)
print(cross_table_by_gender_with_generation)
print(cross_table_by_device)
```

성별을 기준으로 변화를 살펴보면 아래의 테이블을 확인할 수 있습니다.

![cross_table_by_gender](/images/data-analysis/cross_table_by_gender.png)

다른 테이블 역시 위와 같은 형태로 확인할 수 있습니다. 성별과 세대를 기준으로 변화를 확인하는 것 역시 가능합니다.
위 코드처럼 기준값을 배열의 형태로 넘겨주면 됩니다.

![cross_table_by_gender_with_generation](/images/data-analysis/cross_table_by_gender_with_generation.png)

성별과 연령 기준으로 모두 확인해보아도 모두 감소세이지만 전체적으로 비율이 거의 비슷하여 특정 유저층을 발견하기 어렵습니다. 하지만
단말기를 기준으로 분석을 진행했을 때 유의미한 차이를 발견할 수 있습니다. 아래의 테이블을 확인하면 iOS를 사용하는 유저수는 거의
줄어들지 않은 반면, Android를 사용하는 유저수가 크게 감소한 것을 알 수 있습니다.

## 시각화하기

```python
aggregator = {'user_id': 'count'}
user_device_by_date = daily_active_user_info.groupby(
  ['login_date', 'device_type'], as_index=False).agg(aggregator).rename(columns=aggregator)
print(user_device_by_date)

color_by_kind = {'Android': 'blue', 'iOS': 'red'}
for kind in color_by_kind:
    data = user_device_by_date[user_device_by_date['device_type'] == kind]
    plt.plot(data['login_date'], data['count'], color=color_by_kind[kind], label=kind)
plt.legend(loc='upper left')
plt.xticks([])
plt.show()
```

Android 유저가 크게 감소하였기 때문에 시간에 따라 정말로 감소했는지 확인해보도록 하겠습니다. 먼저 날짜와 기기 유형에 따라 집계한 다음 각
기기 유형에 따라 시각화하는 것이 필요합니다. Android는 파란색, iOS는 붉은 색으로 표시했을 때 아래와 같은 그래프를 확인할 수 있습니다.
(x축의 날짜는 너무 지저분하게 표시되는 관계로 제외했습니다.)

![user_device_by_date](/images/data-analysis/user_device_by_date.png)

Android 기종의 접속이 현저히 떨어졌다는 것을 알 수 있습니다. 이를 바탕으로 특정 기종에 대해 버그가 있는 것은 아닌지 탐색을 이어나갈 수 있습니다.
위 분석의 전체 코드는 아래의 링크를 통해 확인할 수 있습니다.

[churn_analysis](https://github.com/chaehwan-oh/analysis-case-study/blob/master/src/churn_analysis.py)

파이썬 코드에 대해 상세히 설명하지 못한 부분이 많습니다. 파이썬이 익숙하지 않은 분은 제 블로그의 Share탭에 있는 포스트들을 참고해주시기 바랍니다.

참고)

비즈니스 활용사례로 배우는 데이터분석:R - 사카마키 류지, 사토 요헤이 지음
