---
layout: post
subclass: post
title: "Code Complete 정리 (1)"
date: 2018-08-12 00:00:00
tags: [programming, must-read]
excerpt: "programming"
disqus: True
---

※ 스티브 맥코넬의 CODE COMPLETE를 읽고 정리한 글입니다.

# CODE COMPLETE 1 부 기초 확립

## 선행조건 - 준비작업의 중요성

준비작업의 가장 중요한 목표는 위험 축소다. ('준비작업의 중요성')

### 가장 흔히 발생하는 위험 요소

1.  불충분한 요구사항
2.  잘못된 프로젝트 계획

집을 지을 때는 기초 공사를 시작하기 전에 설계도와 청사진을 만든다. 기술적인 계획 수립은소프트웨어에서도 똑같이 중요하다.

## 문제 정의

구현에 들어가기 앞서 완료해야 하는 첫 번째 선행 조건은 시스템이 해결해야 하는 문제를 명확히 기술하는 것이다.

문제 정의는 반드시 사용자의 언어로 작성해야 하며 문제는 사용자 관점에서 기술해야 한다. 대개 컴퓨터 전문 용어로 작성하는 것은 안 된다. 최고의 해결책이 컴퓨터 프로그램이 아닐 수도 있다.

## 왜 명시적인 요구사항이 필요한가?

1.  사용자가 원하는 것이 무엇인지를 알 수 있다.
2.  논쟁을 피하게 해준다.
3.  개발을 시작하고 난 후 변경 사항을 최소화하는 데 도움을 준다.

## 선행조건에 소요되는 시간

일반적으로 제대로 진행되는 프로젝트는 요구사항과 아키텍처, 사전 계획 수립을 위해서 전체 노력의 10%에서 20% 정도와 전체 시간의 20%에서 30% 정도를 투자한다.

## 구현 시 결정해야 할 핵심사항

프로그래밍 언어의 선택: 구현할 시스템에 사용할 개발 언어에 민감할 수 밖에 없는 이유는 구현을 시작할 때부터끝날 때까지 언어가 상당히 많은 영향을 주기 때문이다.

1.  생산성
2.  개발자의 사고에 영향
