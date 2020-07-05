---
date: 2020-07-05 18:30:00 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---

이번 포스트에서는 데스크톱에 PySide2로 Qt App을 생성해 보자.

# Create Basic Pyside2 App
## 1. Basic App
### 소스코드 및 실행
**app1.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QWidget

app = QApplication(sys.argv)
win = QWidget()
win.show()
sys.exit(app.exec_())
```

위 코드작성이 끝나면 아래 코드로 실행할 수 있다.
```
python app1.py
```
또는
```
python3 app1.py
```

{% include figure image_path="/assets/images/2020-07-05-pyside2-create-app-img1.png" caption="window에서 app1.py 실행 후 모습" %}

### 살펴보기
```python 
import sys
```  
  커맨드라인 인수(arguments)를 받아들이는데에 사용된다.

```python
from PySide2.QtWidgets import QApplication, QWdiget
```  
  어플리케이션 구동에 필요한 `QApplication`클래스는 핸들러이고, 구성할 위젯 `QWdiget` 클래스는 아무것도 없는 텅빈 GUI위젯으로 둘다 QtWidget모듈에서 불러온다. 물론 `from PySide.QtWidgets import *` 와 같은 방식도 사용 할 수 있지만, 이는 명시적이지 않을 뿐 만 아니라 불필요한 객체를 불러오는 경우 빌드단계에서 성능저하를 불러오기도 한다.
  
**Main Modules in Qt**  
Qt의 메인 모듈은 `QtWidgets`, `QtGui`, `QtCore`로 볼 수 있다.
{: .notice--info}

```python
app = QApplication(sys.argv)
```  
  어플리케이션당 하나의 `QApplication`객체만 필요하다. `sys.argv`를 전달하여 command line 에 arguments등을 전달한다. 만약 command line 인수를 사용하지 않는다면 `QApplication([])`로 전달하여도 무방하다. (인수로 스타일/스타일 시트를 커맨드라인으로 전달 하기도 함)
  
```python
win = QWidget()
win.show()
```
  `QWidget`객체를 생성한다. 그리고 `.show()`를 통해 표시한다.

**모든 탑레벨 위젯은 윈도우다**
Qt는 최상위레벨 위젯을 상속하여 다음 레벨의 위젯을 구성한다. 그런데 최상위 레벨의 위젯은 모두 창이다. 또한 기본적으로 `parent`가 존재하지 않으며 레이아웃이나 위젯에 서로가 서로에게 속박되어 있지 않다. 이 말은 어떠한 위젯을 사용하더라도 창을 만들 수 있다는 뜻이다.
{: .notice--info}

**부모 없는 위젯은 항상 히든상태**
위젯은 부모가 없으면 기본적으로 숨은 상태이다. 따라서 `.show()`를 통해서 항상 구동해줘야한다.
{: .notice--warning}

```sys.exit(app.exec_())```
`app.exec_()`로 이벤트 루프를 시작한다. `sys.exit()`는 이 루프가 정상적으로/비정상적으로 끝나는 것을 감지하는데 `sys.exit(0)`는 정상종료, 그 외의 non-zero exit는 비정상종료로 간주되어 error를 일으킨것으로 간주한다.

#### 이벤트루프란?
물론 이벤트 루프에 관한 개념이 없더라도 간단한 GUI프로그래밍을 하는 것은 큰 문제가 아닐지도 모른다. 하지만 중심을 관통하는 키 컨셉들을 이해해야, 제대로 된 Qt app을 구성할 수 있다. 

이벤트 루프의 핵심은 `QApplication` 클래스에 있다. App이 기능하기위해서 꼭 필수적이며 오직 하나만 필요하다. 이 객체는 모든 GUI와 사용자간의 모든 상호작용을 관장한다.


{% include figure image_path="/assets/images/2020-07-05-pyside2-create-app-img2.png" caption="window에서 app1.py 실행 후 모습" %}

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2 [Official Link](www.learnpyqt.com){: .btn .btn--inverse}
