---
date: 2020-07-05 00:14:01 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---
PySide2를 공부하기 전에 간단한 GUI의 역사를 알아보자. 

# PySide2 Intro
## Gui의 간단한 역사
* 1960, 스탠포드의 NLS(oN-Line System)이 마우스와 창이라는 컨셉을 처음 도입하였고 1968년에 이를 공식 시연함.
* 1973, Xerox PARC Smalltalk system GUI 가 1973년도에 GUI의 **WIMP**(창, 메뉴, 라디오버튼, 체크박스, 아이콘과 같은 것)을 도입하여 기틀을 다짐

**WIMP**  
Windows, Icon, Menu, Pointing device의 두문자
{: .notice}

* 1979, PERQ 웍스테이션이라는 첫 GUI 상용시스템 등장.
* 1983-1985, 이에 영감을 받은 애플의 Lisa가 등장. (Atari, Amiga등도 참여) 이어 UNIX가 등장하고 85년에 Windows가 등장하였다.
* 초창기 GUI는 호환가능한 소프트웨어가 없고, 하드웨어가 비싸 홈유저들에게 바로 흥행하지는 못하였다.
* 그러나, GUI는 컴퓨터를 다루기에 좀더 선호되는 방식이었고, WIMP가 표준으로 자리잡아가게 되었다.

**Desktop UI를 Window가 아닌 House로?**  
창이라는 컨셉을 대체할려는 노력이 없었던 것은 아니었다. 1995년 마이크로소포트의 밥은 집을 UI로 만들었던 전례가 있다.
{: .notice--info}

* 당시의 GUI에서 지금까지 거의 변한 것이 없다. 윈도우스 95(1995), Mac OS X(2001), GNOME Shell(2011), 윈도우 10(2015)까지.
* 그러나 혁신은 모바일에서 등장했다. 마우스는 터치로 대체되었고, 창은 풀스크린 app으로 변모하였다.
* 하지만 우리가 매일을 일을 하는데 있어서 아직도 데스크탑 컴퓨터의 사용으로 이루어지고 있고, WIMP는 40년동안 생존해 왔으며,
앞으로도 그럴 예정으로 보여진다.

## 결론(개인적인 생각)
일반인에게 프로그래밍을 한다고 말하면, 대부분의경우 GUI어플리케이션을 떠올리기 마련이다.
Python은 현재 1 티어 프로그래밍 언어라고 생각됨에도 불구하고 Python으로 GUI프로그래밍을 막상 하려고하면
Java나 C-Family에 비해서 Desktop app이 많이 약하다고 생각한다.
비록 Lisence문제가 상용프로그램을 만드는데 있어서 불편한 것은 사실이나, 
Python으로 완성도 높은 GUI프로그래밍을 하기위해서는 Qt가 가장 적합해보인다.
PyQt와 PySide2가 모두 Qt를 이용하는 것이지만,
PyQt를 사용하게되면 이중 Liscence문제가 생기므로
차라리 Qt에서 공식적으로 지원하는 PySide2를 이용하는 편이 더 좋아보인다.

# 참고
**For details, go to**
* [Martin Fitzpatrick](www.learnpyqt.com){: .btn .btn--inverse}
