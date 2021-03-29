---
layout: post
subclass: post
navigation: True
title: "도커 사용해보기"
date: 2019-12-08
tags: [programming, docker, devOps]
excerpt: "도커를 사용하기 위한 기본 사용법을 살펴봤습니다."
disqus: True
---

# Intro

```
/usr/bin/google-chrome-stable: error while loading shared libraries: libgconf-2.so.4: cannot open shared object file: No such file or directory
...
libgconf-2.so.4 => not found
libXss.so.1 => not found
libatk-1.0.so.0 => not found
libgtk-3.so.0 => not found
libgdk-3.so.0 => not found
libgdk_pixbuf-2.0.so.0 => not found
```

프로그램 실행을 위해 chromedriver를 설치하던 중 위와 같은 에러 메시지를 만났습니다. 의존성 있는 라이브러리를 찾아가며 설치하고, 필요한 경우 프로그램을 빌드해가며
해결했지만 은근하게 손이 많이 가는 작업이었습니다. 근래 이렇게 로컬에서 문제 없이 잘 실행되는데 운영체제와 환경이 달라지니 번거로운 작업들이 계속해서 생기는 경우가 제법
있었습니다. 도커를 활용했으면 이런 문제가 없었을텐데 하는 생각이 들어 익혀서 사용해보기로 했습니다.

# 도커란?

```
Docker provides a way to run applications securely isolated in a container, packaged with all its dependencies and libraries.
```

도커의 공식문서에 있는 소개 문장입니다. 애플리케이션을 컨테이너 내에서 모든 의존성 있는 패키지와 라이브러리와 함께 실행할 수 있도록 해준다고 합니다. 간단하게만 설명하면
프로세스를 격리된 환경에서 실행하여 프로세스의 의존성 문제를 해결해주는 기술입니다. 자세한 설명은 아래 링크와 공식문서를 참조하시길 바랍니다.

[초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
[Docker overview](https://docs.docker.com/engine/docker-overview/)

# 일단 사용해보기

도커에 대해 처음 들었을 때 어떻게 도커가 의존성 문제를 해결하는지 제대로 이해하고 싶었습니다. 하지만 도커를 이해하기 위해 cgroups, namespaces 등의 각 개념을 이해하려니
도커를 써보지도 못했는데 벌써 지치는 문제가 생겼습니다. 그래서 일단 사용해보기로 했습니다.

간단히 node.js 애플케이션을 container로 실행해보기로 했습니다.

## 1. node.js 어플리케이션 작성하기

먼저 아래 두 파일을 작성했습니다. package.json의 경우 npm init 명령어로 바로 생성할 수 있습니다. 필요한 부분을 제외하고는 제거했습니다.

package.json)

```json
{
  "dependencies": {
    "express": "*"
  },
  "scripts": {
    "start": "node index.js"
  }
}
```

index.js)

```javascript
const express = require("express");

const app = express();

app.get("/", (req, res) => {
  res.send("Hi there!");
});

app.listen(8080, () => {
  console.log("Listening on port 8080");
});
```

## 2. Dockerfile 작성하기

다음으로 Dockerfile을 작성했습니다. 도커는 도커 이미지에 컨테이너의 상태를 저장한 다음, 이를 바탕으로 컨테이너를 생성하여 실행합니다.
도커 이미지를 생성하기 위해서는 Dockerfile을 작성해야 합니다. 파일명을 Dockerfile로 하여 파일을 생성하시면 됩니다. 도커는 이 Dockerfile의
명령어를 하나씩 읽어 도커 이미지를 생성합니다.

그러므로 컨테이너 실행을 위해 가장 먼저 필요한 것은 Dockerfile입니다. Dockerfile에 대해서는 아래 링크를 참고하시기 바랍니다.

[Dockerfile 작성하기](https://docs.docker.com/engine/reference/builder/)

작성한 Dockerfile을 하나씩 파악해보겠습니다.

Dockerflle)

```Dockerfile
FROM node:alpine

WORKDIR /usr/app

COPY ./ ./
RUN npm install

CMD ["npm", "start"]
```

FROM 명령어는 어떤 컨테이너 이미지를 가지고 와서 빌드할 것인지를 결정하는 명령어입니다. FROM 명령어 뒤에 입력한 이미지를 도커 허브(Docker hub)라고 하는
컨테이너 이미지가 모여있는 이미지 저장소에서 불러옵니다. 소스 코드 저장소인 Github에서 소스 코드를 불러오는 것처럼 컨테이너 이미지를 불러온다고 보시면 됩니다.
(Docker hub에서 이미지를 불러올 때 먼저 로컬에 이미지가 저장되어 있는지 확인하고, 없으면 Docker hub에서 이미지를 불러옵니다.)

아래의 링크를 보시면 Docker hub의 node 이미지에 대한 설명을 확인하실 수 있습니다.

[Docker hub node image](https://hub.docker.com/_/node)

```
FROM <image>[:<tags]
```

이미지명과 태그명을 명시하여 이미지를 불러옵니다. 위 도커 허브의 노드(node) 이미지 링크에서 이미지의 태그(tags)를 확인하실 수 있습니다. FROM 명령어에 대한 자세한 설명은
아래 링크에서 확인하실 수 있습니다.

[Dockerfile FROM](https://docs.docker.com/engine/reference/builder/#from)

WORKDIR 명령어는 일단 뛰어넘고 바로 COPY 명령어를 살펴보겠습니다. COPY 명령어는 로컬 파일시스템에 있는 파일들을 컨테이너의 파일시스템에 옮기는 명령어입니다. node.js를
이미 사용하신 분들은 알겠지만 npm install은 node.js의 패키지 매니저인 npm을 활용하여 의존성 있는 모듈들은 설치하는 명령어입니다. 하지만 이 명령어를 실행하기 위해서는
의존성을 명시하고 있는 package.json 파일이 필요합니다. 하지만 package.json은 로컬 파일시스템에 존재하고 container 내부의 파일시스템에 존재하지 않습니다. 그러므로
package.json을 컨테이너 내부의 파일시스템으로 옮겨주어야 합니다. 위 경우 편의상 현재 디렉토리 내부의 모든 파일을 container 내부의 파일시스템으로 옮겨주었습니다.

뛰어 넘은 WORKDIR 명령어는 working directory를 지정해주는 명령어입니다. COPY 명령어 실행 시 컨테이너 내부 파일시스템의 root로 파일이 옮겨집니다.
이경우 혹시 root의 파일을 덮어쓰는 문제가 생길 수 있으므로 WORKDIR로 working directory를 지정하여 해당 디렉토리에 파일을 옮겨줍니다.

[Dockerfile COPY](https://docs.docker.com/engine/reference/builder/#copy)
[Dockerfile WORKDIR](https://docs.docker.com/engine/reference/builder/#workdir)

그 다음 RUN 명령어입니다. RUN 뒤에 명시한 명령어를 실행하는 명령어입니다. npm install 명령어를 실행하여 필요한 express 모듈을 설치해주었습니다.

[Dockerfile RUN](https://docs.docker.com/engine/reference/builder/#run)

마지막으로 CMD 명령어입니다. CMD 명령어는 컨테이너가 실행될 때 어떤 명령어가 실행되도록 할 것인가?를 결정하는 명령어입니다. 컨테이너가 시작될 때 실행되는 명령어를
지정하는 것이므로 딱 하나의 CMD만이 Dockerfile에 있을 수 있습니다.

[Dockerfile CMD](https://docs.docker.com/engine/reference/builder/#cmd)

## 3. 이미지 생성하기(build)

Dockerfile을 작성한 다음으로는 이미지를 생성해줄 차례입니다. -t 옵션을 꼭 붙여주어야 하는 것은 아니지만 -t 옵션을 붙여 태깅하지 않으면
image ID로 도커 컨테이너를 실행해야 하는 번거로움이 있습니다. 도커 데스트탑에서 로그인한 다음 아래 형식에 따라 명령어를 실행하시면 이미지를
생성할 수 있습니다.

```
docker build -t <docker ID>/<image>:<version>
ex) docker build -t ta555/simpleweb
```

[docker build](https://docs.docker.com/engine/reference/commandline/build/)

생성된 이미지는 터미널에서 docker images 명령어를 입력하여 확인하실 수 있습니다.

## 4. 컨테이너 실행하기

마지막으로 컨테이너를 실행할 단계입니다. 아래 명령어를 실행하면 컨테이너를 실행하실 수 있습니다.

```
docker run <image>
ex) docker run ta555/simpleweb
```

하지만 컨테이너 실행 후 브라우저에서 localhost:8080으로 요청을 보내면 '사이트에 연결할 수 없음' 메세지가 뜨는 것을 확인하실 수 있습니다.
이유는 컨테이너가 아닌 로컬 머신으로 요청을 보내는 것이기 때문입니다. 컨테이너로 요청을 보내려면 포트(port)를 매핑해주는 과정이 필요합니다.
8080 포트로 요청을 보내면 컨테이너 8080 포트로 요청을 포워딩해주는 것입니다. 이는 Dockerfile에 명시해주는 것이 아니라 도커 컨테이너를
실행할 때 명시해줍니다. 아래와 같이 실행하면 됩니다.

```
docker run -p <local port>:<container port> <image>
ex) docker run -p 8080:8080 ta555/simpleweb
```

이렇게 실행한 다음 브라우저에서 localhost:8080으로 요청을 보내면 Hi there! 메세지를 확인하실 수 있습니다.

[docker run](https://docs.docker.com/engine/reference/run/)

# 마치며

간단한 사용법을 파악했으니 이어서 바로 도커의 동작원리까지 바로 정리해보려고 했지만, 한 번 끊어가려고 합니다. 이어서 docker compose를 살펴보고 docker
container 기술의 동작원리를 차근히 살펴보려고 합니다.
