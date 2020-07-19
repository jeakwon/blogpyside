---
date: 2020-07-20 01:20:00 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---
PySide2의 윈도우 생성에 대해서 알아보자
[소스코드](https://github.com/jeakwon/pyside2/tree/master/07_windows){: .btn .btn--primary}

# 윈도우에 대하여
[전편](https://jeakwon.github.io/pyside2/pyside2-windows/)에서 다룬 대화상자(Dialog)는 특별한
종류의 모달 윈도우를 만든 것인데, 이러한 특수목적 창이 아닌, 일반적인 위젯을 다양하게 구성할 수 있는
창을 생성하는 법에대해서 알아보자.
기본적으로 Qt 위젯들은 전부 개별적으로 윈도우를 구성할 수 있다는 사실을 염두해 둔다면 좋을 것이다.
또한 메인윈도우는 정말 단하나의 메인윈도우의 개념이라기보단, 툴바/메뉴바/상태바 등을 제공해주는 윈도우
이므로 새로운 생성된 창에서도 그러한 기능들이 필요하다면 메인윈도우도 사용할 수 있다.

# 1. Create new window
## 소스코드
**win1.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton, QWidget, QVBoxLayout
from random import randint

class NewWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("새창"+str(randint(1,100)))
        self.resize(250, 100)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.btn1 = QPushButton("create_without_reference")
        self.btn1.clicked.connect(self.create_without_reference)
        self.btn2 = QPushButton("create_with_reference")
        self.btn2.clicked.connect(self.create_with_reference)

        layout = QVBoxLayout()
        layout.addWidget(self.btn1)
        layout.addWidget(self.btn2)

        widget = QWidget()
        widget.setLayout(layout)

        self.setCentralWidget(widget)
    
    def create_without_reference(self):
        w = NewWindow()
        w.show()

    def create_with_reference(self):
        self.w = NewWindow()
        self.w.show()

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

## 결과 및 설명
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/07_windows/win1a.gif){: .align-center}
```python
    def create_without_reference(self):
        w = NewWindow()
        w.show()
```
- 새로운 창은 단순히 생성만해서는 안되고 `.show()`로 보여줘야한다.
- 하지만 위 코드로 창을 생성해보면, 순식간에 사라지게 된다. 하지만 새로 생성된 창은 레퍼런스가 없어, 파이썬이
즉각적으로 클린업하여 창은 소멸되게 된다. (아무래도 레퍼런싱을 하지 않으면 이벤트루프에서 생성된 창을 관리하지
못하는 것으로 생각됨)

![](https://raw.githubusercontent.com/jeakwon/pyside2/master/07_windows/win1b.gif){: .align-center}
```python
    def create_with_reference(self):
        self.w = NewWindow()
        self.w.show()
```
- 따라서 `self.w`로 메인윈도우에 생성된 창을 레퍼런스를 하게되면 아까와는 다르게 지속되는 창을 볼 수 있다.

# 2. Create new VS Show existing windows
어플리케이션에서 메인윈도우 이외의 창을 다양한 목적으로 조작 할 수 있다: 1) 매번 새롭게 창을 생성하거나 또는
2) 이미 만들어진 창을 보여주고 닫거나. 
## 소스코드
**win2.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton, QWidget, QVBoxLayout
from random import randint

class NewWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("새창"+str(randint(1,100)))
        self.resize(250, 100)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.w1 = None
        self.btn1 = QPushButton("create_new_window")
        self.btn1.clicked.connect(self.create_new_window)

        self.w2 = NewWindow()
        self.btn2 = QPushButton("show_existing_window")
        self.btn2.clicked.connect(self.show_existing_window)

        layout = QVBoxLayout()
        layout.addWidget(self.btn1)
        layout.addWidget(self.btn2)

        widget = QWidget()
        widget.setLayout(layout)

        self.setCentralWidget(widget)
    
    def create_new_window(self):
        if self.w1 is None:
            self.w1 = NewWindow()
            self.w1.show()
        else:
            self.w1.close()
            self.w1 = None
    
    def show_existing_window(self):
        if self.w2.isVisible()==False:
            self.w2.show()
        elif self.w2.isVisible()==True:
            self.w2.close()

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```
## 결과 및 설명
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/07_windows/win2a.gif){: .align-center}
```python
    def create_new_window(self):
        if self.w1 is None:
            self.w1 = NewWindow()
            self.w1.show()
        else:
            self.w1.close()
            self.w1 = None
```
- 창을 닫는데 필요한 메소드는 `.close()`이다. 여기서 창을 새롭게 생성하는 것이 매번 윈도우 숫자가 바뀌는 것을
통해서 알 수가 있다. 이는 윈도우를 닫은 뒤 `self.w1 = None`으로 레퍼런스를 초기화 해줬기 때문이다.
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/07_windows/win2b.gif){: .align-center}
```python
    def show_existing_window(self):
        if self.w2.isVisible()==False:
            self.w2.show()
        elif self.w2.isVisible()==True:
            self.w2.close()
```
- 미리 메인 윈도우에서 생성된 `self.w2 = NewWindow()`를 이번에는 버튼을 누를때마다 보여주기/닫기가 되도록 만들었다.
- `.isVisible()`를 통해서 기존에 존재하는 창이 보이는 상태인지 아닌지를 알 수 있다.

# 3. Create windows with/without signals
## 소스코드
**win3.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QCheckBox, QWidget
from random import randint

class NewWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("새창"+str(randint(1,100)))
        self.resize(250, 100)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.chkbx1 = QCheckBox("show_window_without_signal")
        self.chkbx1.clicked.connect(self.show_window_without_signal)
        self.chkbx2 = QCheckBox("show_window_with_signal")
        self.chkbx2.clicked.connect(self.show_window_with_signal)

        layout = QVBoxLayout()
        layout.addWidget(self.chkbx1)
        layout.addWidget(self.chkbx2)

        widget = QWidget()
        widget.setLayout(layout)

        self.setCentralWidget(widget)
        self.w = NewWindow()
    
    def show_window_without_signal(self):
        if self.w.isVisible()==False:
            self.w.show()
        elif self.w.isVisible()==True:
            self.w.close()
    
    def show_window_with_signal(self, checked):
        if checked:
            self.w.show()
        else:
            self.w.close()

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

## 결과 및 설명
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/07_windows/win3.gif){: .align-center}
```python
    def show_window_without_signal(self):
        if self.w.isVisible()==False:
            self.w.show()
        elif self.w.isVisible()==True:
            self.w.close()

    def show_window_with_signal(self, checked):
        if checked:
            self.w.show()
        else:
            self.w.close()
```
- `show_window_with_signal(self, checked)`를 통해서 체크박스가 체크가 되어있는지 아닌지를 알 수 있다. 이를
통해서 시그널이 발생되었을 때 창을 보여주고 닫을 수 있다.

# 4. Delete windows on close
## 소스코드
**win4.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QCheckBox, QWidget
from PySide2.QtCore import Qt
from random import randint

class NewWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("새창"+str(randint(1,100)))
        self.resize(250, 100)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.w1 = NewWindow()
        self.chkbx1 = QCheckBox("without_delete_on_close")
        self.chkbx1.clicked.connect(self.without_delete_on_close)

        self.w2 = NewWindow()
        self.chkbx2 = QCheckBox("with_delete_on_close")
        self.chkbx2.clicked.connect(self.with_delete_on_close)

        layout = QVBoxLayout()
        layout.addWidget(self.chkbx1)
        layout.addWidget(self.chkbx2)

        widget = QWidget()
        widget.setLayout(layout)

        self.setCentralWidget(widget)
    
    def without_delete_on_close(self, checked):
        if checked:
            self.w1.show()
        else:
            self.w1.close()
    
    def with_delete_on_close(self, checked):
        self.w2.setAttribute(Qt.WA_DeleteOnClose)
        if checked:
            self.w2.show()
        else:
            self.w2.close()

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```
## 결과 및 설명
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/07_windows/win4.gif){: .align-center}
```python
    def without_delete_on_close(self, checked):
        # ...
    
    def with_delete_on_close(self, checked):
        self.w2.setAttribute(Qt.WA_DeleteOnClose)
        # ...
```
- 여기서 `.setAttribute(Qt.WA_DeleteOnClose)`를 이용하면, `.close()`메소드가 발동 되었을때, 객체를 삭제하게 된다.
이는 다시말하면 `.close()`는 창을 삭제하지 않고, 단순히 창을 숨겨주고 있다는 것이다. 메모리 관리측면에서 볼 때,
일회성으로 생성되고 사라지게 될 창에 이용하면 좋을 것으로 보인다.

# 5. close vs hide (feat. setVisible)
## 소스코드
**win5.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QCheckBox, QWidget
from random import randint

class NewWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("새창"+str(randint(1,100)))
        self.resize(250, 100)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.w = NewWindow()

        self.chkbx1 = QCheckBox("show_and_close")
        self.chkbx1.clicked.connect(self.show_and_close)

        self.chkbx2 = QCheckBox("show_and_hide")
        self.chkbx2.clicked.connect(self.show_and_hide)

        self.chkbx3 = QCheckBox("set_visible")
        self.chkbx3.clicked.connect(self.set_visible)

        layout = QVBoxLayout()
        layout.addWidget(self.chkbx1)
        layout.addWidget(self.chkbx2)
        layout.addWidget(self.chkbx3)

        widget = QWidget()
        widget.setLayout(layout)

        self.setCentralWidget(widget)
    
    def show_and_close(self):
        if self.w.isVisible():
            self.w.close()
        else:
            self.w.show()
        
    def show_and_hide(self):
        if self.w.isVisible():
            self.w.hide()
        else:
            self.w.show()
    
    def set_visible(self):
        if self.w.isVisible():
            self.w.setVisible(False)
        else:
            self.w.setVisible(True)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```
## 결과 및 설명
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/07_windows/win5.gif){: .align-center}
```python
    def show_and_close(self):
        if self.w.isVisible():
            self.w.close()
        else:
            self.w.show()
        
    def show_and_hide(self):
        if self.w.isVisible():
            self.w.hide()
        else:
            self.w.show()
    
    def set_visible(self):
        if self.w.isVisible():
            self.w.setVisible(False)
        else:
            self.w.setVisible(True)
```
- 위 코드에서 `.hide()` = `.setVisible(False)` 의 관계이며, `.show()` = `.setVisible(True)`의 관계이다.
- `.close()`와 `.hide()`의 경우 특별히 `.setAttribute(Qt.WA_DeleteOnClose)`를 활성화 시키지 않으면 똑같이 행동한다.

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2  
[Official Link](https://www.learnpyqt.com/){: .btn .btn--inverse}
