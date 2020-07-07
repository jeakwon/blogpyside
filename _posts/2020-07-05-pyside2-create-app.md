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
PySide2 앱을 만들어 보고, 이벤트 루프에대해서, 그리고 왜 QMainWindow를 사용하는지 알아보자
[소스코드](https://github.com/jeakwon/pyside2/tree/master/1_create_app){: .btn .btn--primary}

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

{% include figure image_path="/assets/images/2020-07-05-pyside2-create-app/img1.png" caption="window에서 app1.py 실행 후 모습" %}

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

```python
sys.exit(app.exec_())
```
`app.exec_()`로 이벤트 루프를 시작한다. `sys.exit()`는 이 루프가 정상적으로/비정상적으로 끝나는 것을 감지하는데 `sys.exit(0)`는 정상종료, 그 외의 non-zero exit는 비정상종료로 간주되어 error를 일으킨것으로 간주한다.

### 이벤트루프란?
물론 이벤트 루프에 관한 개념이 없더라도 간단한 GUI프로그래밍을 하는 것은 큰 문제가 아닐지도 모른다. 하지만 중심을 관통하는 키 컨셉들을 이해해야, 제대로 된 Qt app을 구성할 수 있다. 

이벤트 루프의 핵심은 `QApplication` 클래스에 있다. App이 기능하기위해서 꼭 필수적이며 오직 하나만 필요하다. 이 객체는 모든 GUI와 사용자간의 모든 상호작용을 관장한다.


{% include figure image_path="/assets/images/2020-07-05-pyside2-create-app/img2.png" caption="window에서 Qt에서의 이벤트 " %}

**QApplication Class**  
@ `Qapplication`은 Qt Event Loop을 잡아둔다.  
@ 하나의 `QApplication` 객체만 필요하다.  
@ 액션이 취해지기 전까지 이벤트루프는 기다린다.  
@ 언제든 단 하나의 이벤트 루프만 존재한다.
{: .notice--info}

## 2. QMainWindow
### QMainWindow를 사용하는 이유
예를들어 버튼 위젯을 만들고 싶다고 한다면 위에서 작성한 코드에서 `QWidget`을 `QPushButton`으로 바꾸는 것만으로 PushButton App을 볼 수 있다. 
**app2.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QPushButton

app = QApplication(sys.argv)
win = QPushButton("클릭")
win.show()
sys.exit(app.exec_())
```
매우 간단하지만, 사실 그다지 쓸모있지는 않다. 왜냐하면 단순히 하나의 컨트롤만 가능한 UI를 만드는 것이 목적이
아니기 때문이다. 따라서 서로 다른 기능을 가진 다양한 위젯을 하나에 담을 공간이 필요한데, 이를 레이아웃`layout`
이라 한다. QWidget라는 텅 빈 위젯에 다양한 UI들을 담을 수도 있겠지만, 이미 Qt는 이러한 레이아웃 역할을 해줄
`QMainWindow`를 제공하고 있다.

**QMainWindow**  
QMainWindow에는 미리 만들어진 여러 위젯들이 이미 포함되어 있다.  
Toolbars, Menus, Statusbars, Dockable Widgets, etc.
{: .notice--info}

### QMainWidnow의 기본형태
#### 소스코드
**app3.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow

app = QApplication(sys.argv)
win = QMainWindow()
win.show()
sys.exit(app.exec_())
```
기본적으로 큰 틀은 전혀 바뀐게 없지만, QMainWindow 클래스를 커스터마이징을 통해 버튼위젯을 포함시켜보자

### 커스터마이징 - 버튼추가
#### 소스코드 및 실행 결과
**app4.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("앱")

        btn = QPushButton("클릭")

        self.setCentralWidget(btn)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
sys.exit(app.exec_())
```

{% include figure image_path="/assets/images/2020-07-05-pyside2-create-app/img3.png" caption="app4.py 실행 결과" %}

#### 살펴보기
```python
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton
```
대부분의 일반적인 위젯은 `QtWidgets` 에서 import 된다.

```python
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
```
`QMainWindow`를 상속받는 새로운 `MainWindow`클래스를 만들어 커스터마이징을 하는 경우,
항상 상위클래스`super()`를 발동`__init__`시켜줘야 한다.

```python
        self.setWindowTitle("앱")
        btn = QPushButton("클릭")
```
실행된 창의 이름을 정해주고, 새로운 버튼 위젯을 생성한다.

```python
        self.setCentralWidget(btn)
```
MainWindow의 중심위젯에 생성한 버튼 위젯을 지정해 준다.


### 커스터마이징 - 사이즈조절
#### 소스코드 및 실행 결과
**app5.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton
from PySide2.QtCore import QSize

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("앱")
        btn = QPushButton("클릭")
        self.setCentralWidget(btn)        
        
        self.setFixedSize(QSize(200, 400)) 


app = QApplication(sys.argv)
win = MainWindow()
win.show()
sys.exit(app.exec_())
```

{% include figure image_path="/assets/images/2020-07-05-pyside2-create-app/img4.png" caption="app4.py vs app5.py 실행 결과 비교" %}

#### 살펴보기
```python
from PySide2.QtCore import QSize
```
`QSize`를 이용해 QtWiget의 사이즈 변경시 전달 하는 객체이다. 형식은 `QSize(가로, 세로)`. 

```python
class MainWindow(QMainWindow):
    def __init__(self):
        # ...
        self.setFixedSize(QSize(200, 400)) 
```
위 `QSize`객체를 이용해 `self.setFixedSize()`에 전달하여 창의 크기를 변경해준다.

**사이즈에 관한 사실**  
@ `.setFixedSize()`이외에도 `.setMinimumSize()`, `.setMaximumSize()` 등을 사용할 수 있다.  
@ 어떤 위젯이든 size를 변경할 수 있다.  
@ 위젯의 사이즈를 고정하는 경우엔 화면을 꽉채워도 위젯의 크기는 고정된다.
{: .notice--info}

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2 [Official Link](www.learnpyqt.com){: .btn .btn--inverse}
