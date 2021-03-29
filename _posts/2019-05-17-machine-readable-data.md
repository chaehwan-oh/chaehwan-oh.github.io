---
layout: post
subclass: post
title: "파이썬 프로그래밍 입문(12) machine readable data"
date: 2019-05-17 00:00:12
tags: [python-programming, machine readable data]
excerpt: "파이썬 프로그래밍 입문, machine readable data에 대한 설명입니다."
disqus: True
---

## machine readable data

pdf 형식은 앞서 살펴봤지만 형식을 예측하기 힘들어 다루기 힘듭니다. pdf가 아닌 다른 형식이 있다면 다른 형식의 파일을 이용해 작업하는 것이 더 좋습니다.

이번 내용은 pdf와 같이 사람들이 보기에 편한 형식이 아닌, machine readable data 즉 기계(컴퓨터)에서 다루기 쉬운 형식입니다.

CSV, JSON, XML이 있습니다. 무엇이고 언제 사용하는지 간단하게 보고 넘어가도록 하겠습니다.

- CSV
- JSON
- XML

## CSV

CSV는 데이터의 열을 쉼표로 구분하는 파일입니다. 한 줄이 하나의 레코드를 의미하고 쉼표를 통해 하나의 필드를 나타냅니다. 교재에서 확인하셨겠지만 읽어들이는 방법도 간단합니다.

```
"ID", "NAME"
"1", "chaehwan"
"2", "jinsuk"
```

위와 같은 'test.csv' 파일이 있다고 할 때 아래와 같이 쉽게 읽어들일 수 있습니다.

```python
import csv

csvfile = open('test.csv', 'r')
reader = csv.reader(csvfile)

for row in reader:
    print(row)
```

```
Output

['ID', 'NAME']
['1', 'chaehwan']
['2', 'jinsuk']
```

그럼 언제 왜 csv를 사용하는지 살펴보겠습니다. 엄격한 표 형태의 데이터를 다룰 때 주로 사용합니다. 이 때 csv 파일 형식으로 데이터베이스 등의 애플리케이션에 import 또는 export할 때 사용합니다. 즉, csv 형식의 파일을 읽어 쉽게 데이터베이스에 내용을 담을 수도 있고, 데이터베이스의 내용을 csv 형식의 파일로 내려받는 것도 가능합니다.

## JSON

JSON은 프로그래밍 경험이 이미 있으신 분들은 익숙하겠지만 처음이신 분들은 생소할 수 있는 파일 형식입니다. JavaScript Object Notation의 줄임말입니다. JavaScript 객체 문법으로 구조화된 데이터를 문자열로 나타낸 형식입니다.

```js
{
  "name": "Molecule Man",
  "age": 29,
  "secretIdentity": "Dan Jukes",
  "powers": [
    "Radiation resistance",
    "Turning tiny",
    "Radiation blast"
  ]
}
```

주로 네트워크를 통해 통신할 때 사용됩니다. "lightweight"라는 표현을 사용하는데 XML 등 다른 형식처럼 태그로 감싸거나 하지 않고 간단하게 tree 구조를 나타내고 변환할 수 있기 때문입니다.

## XML

XML은 html처럼 markup language입니다. 즉 태그로 문서의 구조를 나타내는 언어입니다.
정보는 두 군데 들어갈 수 있는데 바로 첫 번째는 태그 사이이고, 두 번째는 속성입니다. XML은 다음과 같은 형태입니다.

```xml
<?xml version="1.0"?>
<data>
  <users>
    <user name="chaehwan">
      <age>27</age>
      <language>python</language>
    </user>
    <user name="jinsuk">
      <age>27</age>
      <language>javascript</language>
    </user>
  </users>
</data>
```

xml.etree.ElementTree 모듈을 꼭 사용해야 하는 것은 아니지만 다음 모듈을 사용하면 아래처럼 쉽게 데이터를 조회할 수 있습니다.

```python
from xml.etree import ElementTree as ET

tree = ET.parse('test.xml')
root = tree.getroot()

users = root.find('users')
for user in users:
    print(user.attrib)
```

XML 역시도 데이터를 전송하는데 사용됩니다. JSON 형식이 웹 앱에서 더욱 적합하다는 평가를 받으면서 조금씩 잊혀지고 있는 상황입니다. 하지만 그래픽 파일이나 바이너리 코딩된 파일에 대해서는 XML 파일이 더욱 적합하다고 하니 아직 사용 용도는 남아있다고 볼 수 있겠습니다.
