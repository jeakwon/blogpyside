---
date: 2020-07-10 01:36:00 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---
PySide2의 레이아웃에 대하여 알아보자
[소스코드](https://github.com/jeakwon/pyside2/tree/master/04_layouts){: .btn .btn--primary}

# 레이아웃이란?
[전편](https://jeakwon.github.io/pyside2/pyside2-widgets/)에서는 위젯의 종류를 알아보았다.
레이아웃은 여러개의 위젯을 하나의 창에 담고 어디에 위치할지를 조율해주는 역할을 한다. Qt에서는
총 네가지 종류의 레이아웃이 존재한다. 가로(x축), 세로(y축), 바둑판식(x&y축) 그리고 중첩식(z축) 
레이아웃. 레이아웃은 아니지만 여러 위젯을 담는데 유용한 탭 위젯도 같이 알아보자.

**Layout 종류**  
@ `QVBoxLayout`: 일자형 세로 박스 레이아웃  
@ `QHBoxLayout`: 일자형 가로 박스 레이아웃  
@ `QGridLayout`: (x, y) 그리드 인덱스를 이용한 레이아웃  
@ `QStackedLayout`: z축으로 여러개가 겹쳐질 수 있는 레이아웃  
@ `QTabWidget`: 조작이 쉬운 `QStackedLayout`처럼 중첩이 가능한 내장 위젯.
.{: .notice--info}

# 1. QVBoxLayout 
`QVBoxLayout`을 이용하면 세로로 위젯을 추가할 수 있다. 위에서 아래로 추가되는 형식이다. 
플레이스홀더로 색깔 위젯을 이용하여 레이아웃의 형태를 알아보자.

## 소스코드
**lay1.py**
```python
from PySide2.QtGui import QColor, QPalette
from PySide2.QtWidgets import QWidget

class Color(QWidget):
    def __init__(self, color):
        super().__init__()
        self.setAutoFillBackground(True)
        palette = self.palette()
        palette.setColor(QPalette.Window, QColor(color))
        self.setPalette(palette)

import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QWidget, QVBoxLayout

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        layout = QVBoxLayout()

        layout.addWidget(Color("red"))
        layout.addWidget(Color("green"))
        layout.addWidget(Color("blue"))
        layout.addWidget(Color("yellow"))

        widget = QWidget()
        widget.setLayout(layout)

        self.setCentralWidget(widget)
        
app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/04_layouts/lay1.png){: .align-center}

## 설명
### 1. 플레이스홀더 위젯
여기서 플레이스홀더 위젯은 기능은 없지만 단순히 그 위치를 잡을 목적으로 생성한(색깔만 입힌) 위젯을 지칭한다.
```python
from PySide2.QtGui import QColor, QPalette
from PySide2.QtWidgets import QWidget

class Color(QWidget):
    def __init__(self, color):
        super().__init__()
        self.setAutoFillBackground(True)
        palette = self.palette()
        palette.setColor(QPalette.Window, QColor(color))
        self.setPalette(palette)
```
먼저 `class Color(QWidget):`로 `QWidget`을 상속받아 새로운 커스텀 위젯을 만든다. 그리고 새로운 위젯의 색깔을 
지정해 주기 위해 `def __init__(self, color):`로 `color`인수를 전달한다. `self.setAutoFillBackground(True)`
로 배경색깔을 채울 수 있게 한다. 다음은 색깔을 지정해 줘야 하는데, 색을 지정하려면 위젯의 `palette`에 접근하여
`palette.setColor(QPalette.Window, QColor(color))`로 색을 `palette`에 전달하고, `self.setPalette(palette)`
로 `palette`를 지정해준다. 그러면 `Color(color)`형태로 인수를 전달하여 레아이웃에 사용 할 위젯을 생성한다.

### 2. 레이아웃에 위젯 추가하기
```python
        layout = QVBoxLayout()
        layout.addWidget(Color("red"))
```
`layout = QVBoxLayout()`로 레이아웃을 등록한다. `.addWidget`으로 위젯을 추가한다.

### 3. 메인윈도우에 레이아웃 등록
```python
        widget = QWidget()
        widget.setLayout(layout)
        self.setCentralWidget(widget)
```
레이아웃을 메인윈도우에 등록하기 위해 `QWidget`을 이용하여 레이아웃을 등록한 뒤 그 위젯을 `.setCentralWidget`을 
해준다. 어떤 위젯을 구성하더라도 2번의 메인 `layout` 구성만 바꾸어 메인윈도우에 등록하면 된다.

# 2. QHBoxLayout

## 소스코드
**lay2.py**
```python
from PySide2.QtGui import QColor, QPalette
from PySide2.QtWidgets import QWidget

class Color(QWidget):
    def __init__(self, color):
        super().__init__()
        self.setAutoFillBackground(True)
        palette = self.palette()
        palette.setColor(QPalette.Window, QColor(color))
        self.setPalette(palette)

import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QWidget, QHBoxLayout

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        layout = QHBoxLayout()

        layout.addWidget(Color("red"))
        layout.addWidget(Color("green"))
        layout.addWidget(Color("blue"))
        layout.addWidget(Color("yellow"))

        widget = QWidget()
        widget.setLayout(layout)

        self.setCentralWidget(widget)
        
app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/04_layouts/lay2.png){: .align-center}

## 설명
2번 색션 설명과 동일 `QVBoxLayout()`만 `QHBoxLayout()`로 수정

# 3. 레이아웃 중첩

## 소스코드
**lay3.py**
```python
from PySide2.QtGui import QColor, QPalette
from PySide2.QtWidgets import QWidget

class Color(QWidget):
    def __init__(self, color):
        super().__init__()
        self.setAutoFillBackground(True)
        palette = self.palette()
        palette.setColor(QPalette.Window, QColor(color))
        self.setPalette(palette)

import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QWidget, QVBoxLayout, QHBoxLayout

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        layout = QHBoxLayout()

        l1 = QVBoxLayout()
        l1.addWidget(Color("red"))
        l1.addWidget(Color("green"))
        l1.addWidget(Color("blue"))
        l1.addWidget(Color("yellow"))
        layout.addLayout(l1)

        l2 = QVBoxLayout()
        l2.addWidget(Color("black"))
        l2.addWidget(Color("white"))
        layout.addLayout(l2)

        l3 = QVBoxLayout()
        l3.addWidget(Color("grey"))
        layout.addLayout(l3)

        layout.setContentsMargins(0, 0, 0, 0)
        layout.setSpacing(20)

        widget = QWidget()
        widget.setLayout(layout)

        self.setCentralWidget(widget)
        
app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/04_layouts/lay3.gif){: .align-center}

## 설명
1. 레이아웃 중첩  
레이아웃을 중첩하는 방법은 레이아웃에 위젯이 아닌 레이아웃을 추가하는 것이다.
각각의 추가된 레이아웃은 레이아웃이나 위젯들을 추가하여 구성을 다양하게 구성 할 수 있다.
편하게 부모레이아웃과 자식레이아웃의 관계로 보면 된다.
```python
layout = QHBoxLayout()
l1 = QVBoxLayout()
l1.addWidget(Color("red"))
layout.addLayout(l1)
```
`layout`은 부모레이아웃으로, `l1`은 자식레이아웃으로 본다면 `.addLayout`을 통해 부모레이아웃에 자식레이아웃을 추가한다.

2. 레이아웃 여유간격주기
레이아웃에 여유간격을 주기 위해서 `.setContentsMargins`과 `.setSpacing`을 사용할 수 있다.  
@ `.setContentsMargins`은 엣지(edge)쪽에 여유를 준다.  
@ `.setSpacing`은 레이아웃간의 간격을 준다.

# 4. QGridLayout
레이아웃 중첩을 통해서 위젯을 다양하게 배치 할 수 있지만, 가로와 세로의 배치만으로는 한계가 있다. 그런 경우 바둑판형태로 
공간을 분할하여 위젯을 배치할 수도 있는데, `QGridLayout`은 그럴 때 사용할 수 있다.

## 소스코드
**lay4.py**
```python
from PySide2.QtGui import QColor, QPalette
from PySide2.QtWidgets import QWidget

class Color(QWidget):
    def __init__(self, color):
        super().__init__()
        self.setAutoFillBackground(True)
        palette = self.palette()
        palette.setColor(QPalette.Window, QColor(color))
        self.setPalette(palette)

import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QWidget, QGridLayout

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
    
        layout = QGridLayout()
        layout.addWidget(Color("red"), 0, 0)
        layout.addWidget(Color("green"), 1, 0)
        layout.addWidget(Color("blue"), 0, 1)
        layout.addWidget(Color("yellow"), 1, 1)
        layout.addWidget(Color("black"), 2, 2)
        layout.addWidget(Color("white"), 100, 0)
        layout.addWidget(Color("grey"), 100, 100)

        widget = QWidget()
        widget.setLayout(layout)
        self.setCentralWidget(widget)

app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/04_layouts/lay4.gif){: .align-center}

## 설명
`QGridLayout`를 사용하는경우 `.addWidget(위젯, 행, 열)`을 통해 원하는 위치에 배치할 수 있다.  
그러나, `layout.addWidget(Color("grey"), 100, 100)`를 보면 해서 사이에 비어있는 공간을 자동으로 채워 주진 
않는다.

# 5. QStackedLayout - 기본
`QStackedLayout`은 요소들의 위치가 서로 중첩될 수 있는 형태의 레이아웃을 제공한다. 물론 원하는 요소를 가장
상단에 오게 만들 수 있다. 예를들면 그래픽을 다룰때, 하나의 오브잭트가 다른 오브잭트위에 그려지게 할 수도 있다.
물론 `QStackedWidget`이라는 위젯도 존재하며, 정확히 `QStackedLayout`과 동작 방식이 똑같으며, `QMainWindow`에 
넣기 위해서 텅빈 `QWidget`을 생성하는 단계를 스킵하고, `.setCentralWidget`로 직접 등록 할 수 있다.

## 소스코드
**lay5.py**
```python
from PySide2.QtGui import QColor, QPalette
from PySide2.QtWidgets import QWidget

class Color(QWidget):
    def __init__(self, color):
        super().__init__()
        self.setAutoFillBackground(True)
        palette = self.palette()
        palette.setColor(QPalette.Window, QColor(color))
        self.setPalette(palette)

import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QWidget, QStackedLayout

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
    
        layout = QStackedLayout()
        layout.addWidget(Color("red"))
        layout.addWidget(Color("green"))
        layout.addWidget(Color("blue"))
        layout.addWidget(Color("yellow"))

        layout.setCurrentIndex(0)
        
        widget = QWidget()
        widget.setLayout(layout)
        self.setCentralWidget(widget)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/04_layouts/lay5.gif){: .align-center}

## 설명
`QStackedLayout`은 여러가지 중첩된 요소들 중에서 선택된 요소를 표시해 준다. 요소를 선택 하는 방법은
`.setCurrentIndex()` 또는 `.setCurrentWidget()`를 이용할 수 있는데, 인덱스(추가한 순서)를 이용하거나
위젯자체를 인수로 제공해 요소를 선택할 수 있다.  

# 6. QStackedLayout - 응용

## 소스코드
**lay6.py**
```python
from PySide2.QtGui import QColor, QPalette
from PySide2.QtWidgets import QWidget

class Color(QWidget):
    def __init__(self, color):
        super().__init__()
        self.setAutoFillBackground(True)
        palette = self.palette()
        palette.setColor(QPalette.Window, QColor(color))
        self.setPalette(palette)

import sys
from PySide2.QtWidgets import (
    QApplication, QMainWindow, QWidget, QPushButton, QHBoxLayout, QVBoxLayout, QStackedLayout)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
    
        layout = QVBoxLayout()
        layout_top = QHBoxLayout()
        layout_bot = QStackedLayout()

        layout.addLayout(layout_top)
        layout.addLayout(layout_bot)

        for i, label in enumerate(["red", "green", "blue", "yellow"]):
            button = QPushButton(label)
            widget = Color(label)

            # def func(x=i):
            #     return layout_bot.setCurrentIndex(x)
            # button.pressed.connect(func)

            button.pressed.connect(lambda x=i: layout_bot.setCurrentIndex(x))

            layout_top.addWidget(button)
            layout_bot.addWidget(widget)
        
        widget = QWidget()
        widget.setLayout(layout)
        self.setCentralWidget(widget)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/04_layouts/lay6.gif){: .align-center}

## 설명
```python
for i, label in enumerate(["red", "green", "blue", "yellow"]):
    # ...
```
`enumerate`를 이용하면 list에 for 문을 돌릴때 index를 부여할 수 있다.

```python
def func(x=i):
    return layout_bot.setCurrentIndex(x)
button.pressed.connect(func)
```
`button.pressed.connect(func)`를 이용해서 버튼의 `.pressed` 시그널과 `func`를 연결하여 중첩된 위젯들사이에서
원하는 위젯을 선택 할 수 있다. 하지만 `func()`, `.setCurrentIndex(i)`의 형태로 함수에 인수를 전달하지 않는 경우
연결된 함수는 마지막 인덱스만을 인식하게 된다. 따라서 `func(x=i)`, `.setCurrentIndex(x)`의 형태로 전달하여, 
`for` 루프가 돌아가는 순간의 인덱스 `i` 값을 사용하게 되므로 제대로 동작하게 된다.

```python
button.pressed.connect(lambda x=i: layout_bot.setCurrentIndex(x))
```
`lambda x=i: layout_bot.setCurrentIndex(x)`는 `func`라는 함수 이름만 없을 뿐 위에서 정의한 함수와 같은 기능을 하고
있다. 이 방법을 이용하면 위젯의 시그널을 중간에 가로채, 우리가 원하는 값을 전달 할 수 있게 된다. (여기서는 인덱스 값, `i`)

# 7. QTabWidget
Qt에는 위에서 만든 버튼과 연동된 `QStackedLayout`와 같은 기능을 하는 내장된 위젯으로 `QTabWidget`을 제공한다.

## 소스코드
**lay7.py**
```python
from PySide2.QtGui import QColor, QPalette
from PySide2.QtWidgets import QWidget

class Color(QWidget):
    def __init__(self, color):
        super().__init__()
        self.setAutoFillBackground(True)
        palette = self.palette()
        palette.setColor(QPalette.Window, QColor(color))
        self.setPalette(palette)

import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QTabWidget

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        widget = QTabWidget()
        widget.setTabPosition(QTabWidget.North)
        
        for label in ["red", "green", "blue", "yellow"]:
            widget.addTab(Color(label), label)
            
        widget.setMovable(True)
        widget.setTabsClosable(True)
        widget.tabCloseRequested.connect(widget.removeTab)

        self.setCentralWidget(widget)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/04_layouts/lay7.gif){: .align-center}

## 설명
`.setTabPosition`는 탭버튼의 위치를 정해준다.  
사용가능한 플래그: `QTabWidget.North`, `QTabWidget.East`, `QTabWidget.West`, `QTabWidget.South`  
`.setMovable`는 탭버튼을 위치를 움직일 수 있게 해 준다.  
`.setTabsClosable`와 `.tabCloseRequested.connect(widget.removeTab)`을 이용하여 탭을 닫을 수 있게 해 준다.

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2 [Official Link](www.learnpyqt.com){: .btn .btn--inverse}
