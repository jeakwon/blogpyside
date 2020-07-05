---
date: 2020-07-05 17:33:00 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---
Qt는 무엇일까? PySide2는 Qt와 어떤관계 일까. 이번 포스트에서 알아보자.

# About Qt
Qt는 크로스 플랫폼 GUI를 생성하기 위한 무료 오픈 소스 위젯 툴킷이다(라고 하지만 라이센스가 복잡하다).  
개발자가 단일 코드 베이스로 어플리케이션을 한번만 개발해도 윈도우, 맥OS, 리눅스, 안드로이드, IOS 등 
여러가지 플랫폼에서 동작하도록 만들 수 있다. 하지만, Qt를 단순한 위젯툴킷의 집합체가 아니라 멀티미디어,
데이터베잇스, 벡터 그래픽, MVC인터페이스 등을 지원하는 하나의 프레임웍이라고 보는 것이 더 정확할 것이다.

Qt는 Eirik Chambe-Eng 과 Haavard Nord가 1991년에 처음 시작하였으며, Trolltech라는 첫 Qt컴퍼니를 
1994년에 설립하였다. Qt는 현재 Qt컴퍼니에서 개발중이며, 지속적인 업데이트와 새로운 기능을 추가하고
있으며, 모바일 플랫폼으로 확장 중이다.

## Qt 와 PySide2
`PySide2`는 Python과 결합한 Qt 툴킷으로, 공식적으로 Qt 에서는 `Qt for Python`으로 불리고 있다.  
만약, PySide2를 통해 앱을 개발한다면, Qt를 통해 개발하는 것과 같은 뜻이다. 왜냐하면, PySide2는
C++로 개발된 Qt라이브러리를 wrapper로 감싸 파이썬으로 사용할 수 있게 한 것이기 때문이다.

따라서, 이것은 C++라이브러리를 Python 인터페이스로 만든 것이기 때문에 naming convention은 PEP8규약을
따르지 않는다. 가장 주목할 만한 것은 `snake_case`가 아니라 `mixedCase`를 이용한다는 것. 일단 코드의
작성 규약을 무엇을 따를 것인지는 본인의 선택에 달려 있겠지만, 파이썬의 다른 라이브러리를 이용하게되면
결국 파이썬 내에서 코드의 규약이 섞이는 일이 발생할 것이다. 만약, 파이썬 규약을 따르게 된다면, 본인의
코드가 어디서 시작하고 어디까지가 PySide2의 코드인지 구별하는 데에 도움이 될 수 도 있다.

PySide2의 전용 문서가 공식적으로 지원은 되고 있지만, Qt문서를 읽는 것이 좀 더 완성에 가까운 경우가 많다. 
만약 Qt의 오리지널 문서를 해석해야하는 경우 아래 공식을 이용하면 도움이 될 것이다.

| Qt             | PySide2        |
|:--------------:|:--------------:|
| Qt::SomeValue  | Qt.SomeValue   | 
| object.exec()  | object.exec_() | 
| object.print() | object.print_()| 

**PEP8 Standards**  
[Official Link](https://www.python.org/dev/peps/pep-0008/){: .btn .btn--inverse}
PEP8 : 파이썬 개선 제안서, 파이썬 코드를 어떻게 구상할 지 알려주는 스타일 가이드
{: .notice--info}



# 참고
This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2
* [Official Link](www.learnpyqt.com){: .btn .btn--inverse}
