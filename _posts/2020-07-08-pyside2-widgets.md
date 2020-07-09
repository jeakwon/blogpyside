---
date: 2020-07-08 03:30:00 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---
PySide2의 위젯에 대하여 알아보자
[소스코드](https://github.com/jeakwon/pyside2/tree/master/3_widgets){: .btn .btn--primary}

# Widgets
[전편](https://jeakwon.github.io/pyside2/pyside2-signal-and-slot/)에서는 시그널&슬롯의 개념에 대하여 
`QPushButton`위젯을 통해서 알아보았다. Qt에는 `QPushButton`이외에도 다양한 위젯들이 존재한다. 위젯이란
**창 안에 존재하는 사용자가 상호작용할 수 있는 UI 컴포넌트**이다. Qt에서는 많은 종류의 위젯이 존재하고
사용자가 새롭게 만들 수도 있다.

## 1. 위젯의 종류
Qt에서 제공하는 위젯의 종류가 정말 많지만, 아래 코드에서는 그 중에서 몇가지 가시적인 것들만
실행해 보았다. 코멘트 처리 된 것을 해제하여 각각의 위젯을 살펴보기 바란다.

### 소스코드
**wid1.py**
```python
import sys
from PySide2.QtWidgets import *

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        layout = QVBoxLayout()
        widgets = [
            QCalendarWidget,
            QCheckBox,
            QColorDialog,
            QComboBox,
            QCommandLinkButton,
            QDateEdit,
            QDateTimeEdit,
            QDial,
            QDoubleSpinBox,
            QFileDialog,
            QFontComboBox,
            QFontDialog,
            QLCDNumber,
            QLabel,
            QLineEdit,
            QProgressBar,
            QPushButton,
            QRadioButton,
            QSlider,
            QSpinBox,
            QTimeEdit,
            QToolButton,
            # QAbstractScrollArea,
            # QAbstractSlider,
            # QAbstractSpinBox,
            # QColumnView,
            # QDesktopWidget,
            # QDialog,
            # QDialogButtonBox,
            # QDockWidget,
            # QErrorMessage,
            # QFocusFrame,
            # QFrame,
            # QGraphicsView,
            # QGroupBox,
            # QInputDialog,
            # QKeySequenceEdit,
            # QListView,
            # QListWidget,
            # QMainWindow,
            # QMdiArea,
            # QMdiSubWindow,
            # QMenu,
            # QMenuBar,
            # QMessageBox,
            # QOpenGLWidget,
            # QPlainTextEdit,
            # QProgressDialog,
            # QScrollArea,
            # QScrollBar,
            # QSplashScreen,
            # QSplitter,
            # QStackedWidget,
            # QStatusBar,
            # QTabBar,
            # QTabWidget,
            # QTableView,
            # QTableWidget,
            # QTextBrowser,
            # QTextEdit,
            # QToolBar,
            # QToolBox,
            # QTreeView,
            # QTreeWidget,
            # QUndoView,
            # QWidget,
            # QWizard,
            # QWizardPage,
        ]

        for w in widgets:
            layout.addWidget(w())
        
        widget = QWidget()
        widget.setLayout(layout)
        
        scroll = QScrollArea()
        scroll.setWidget(widget)

        self.setCentralWidget(scroll)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid1.png){: .align-center}  
위 사진에서 볼 수 있듯, Qt에 내장된 위젯은 형태 뿐만 아니라 기능적으로도
파일접근, 날짜, 폰트, 색깔선택 등 상당히 유용하다. 이는 윈도우뿐만 아니라 MacOS, 리눅스에서도 같은 코드로 구동이 가능하다.

### 설명
Qt의 공식적인 위젯 리스트 및 설명을 보기 위해서는 [Official Link](https://doc.qt.io/qt-5/qtwidgets-module.html){: .btn .btn--primary}을 참고하기 바란다.

## 2. QLabel
`QLabel`은 가장 심플한 위젯 중 하나이다. 한 줄 텍스트를 앱에 표현할 때 사용한다.
```python
from PySide2.QtWidgets import QLabel
widget = QLabel("안녕!")
```
이와 같이 생성 할 때 텍스트를 넣어 표시 할 수 있고,
```python
widget = QLabel("안녕!")
widget.setText("잘가!")
``` 
`.setText` 매소드를 이용하여 이미 표시한 텍스트를 수정 할 수 도 있다.

### 소스코드
**wid2.py**
```python
import sys
from PySide2.QtCore import Qt
from PySide2.QtWidgets import QApplication, QMainWindow, QLabel

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("커피퐁당")
        self.setFixedSize(300, 100)

        label = QLabel("Cafe Fondant")

        font = label.font()
        font.setPointSize(20)
        font.setFamily("Century Gothic")
        font.setItalic(True)
        font.setUnderline(True)

        label.setFont(font)
        label.setAlignment(Qt.AlignHCenter | Qt.AlignVCenter)

        self.setCentralWidget(label)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid2.png){: .align-center}  

### 살펴보기
#### 1. MainWindow 위젯
```python
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("커피퐁당")
        self.setFixedSize(300, 100)
        
        # ...

        self.setCentralWidget(label)
```

위 코드에서 `.setWindowTitle`, `.setFixedSize`, `.setCentralWidget` 이 세 가지는 메인 윈도우 위젯에 관한 것이다. 앱의 타이틀과 사이즈 그리고 메인 위젯을 설정하는 방법.

#### 2. Label 위젯
```python
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        label = QLabel("Cafe Fondant")

        # ...

        label.setFont(font)
        label.setAlignment(Qt.AlignHCenter | Qt.AlignVCenter)
```

`QLabel`로 label 객체를 생성해주고 중략 되었지만, 중간에 font 객체를 생성한 뒤
`.setFont`로 우리가 만든 폰트객체를 지정해준다. 그 후 label의 위치를 정하기 위해
`.setAlignment`로 위치를 지정해준다. 이때 `Qt.AlignHCenter`, `Qt.AlignVCenter`는
`Qt.Core`의 `Qt`오브젝트로 지정해준다. (HCenter: Horizontal Center, 수평중앙, 
VCenter: Vertical Center, 수직중앙)

**Alignment Flags**  
@ `Qt.AlignLeft`: 왼쪽 정렬  
@ `Qt.AlignRight`: 오른쪽 정렬  
@ `Qt.AlignHCenter`: 가로 중앙 정렬  
@ `Qt.AlignJustify`: 양쪽 정렬 (균등하게 매꾸기)  
@ `Qt.AlignTop`: 위쪽 정렬  
@ `Qt.AlignBottom`: 아래쪽 정렬  
@ `Qt.AlignVCenter`: 세로 중앙 정렬  
@ `Qt.AlignVCenter`: 정중앙 정렬  
@ `Qt.AlignLeft | Qt.AlignTop`: 대각선 왼쪽 위(조합 예시)
{: .notice--info}


#### 3. font
```python
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        # ...

        font = label.font()
        font.setPointSize(20)
        font.setFamily("Century Gothic")
        font.setItalic(True)
        font.setUnderline(True)

```
폰트는 이미 `QLabel`객체에 딸려있는 객체로, 생성한 객체에서 `.font()`를 통해 접근할 수 있다. 여기서는 접근한 폰트의 특성 중 네 가지(사이즈, 종류, 이텔릭, 밑줄)를 수정 해 보았다.

## 3. QCheckBox
체크박스 위젯은 말 그대로 체크표시를 할 수 있는 박스이며, 위젯이 자체적으로 체크가 되었는지 되지 않았는지 정보를 담는 역할을 할 수 있다고 보면 된다.
```python
from PySide2.QtWidgets import QCheckBox
widget = QCheckBox("체크박스")
```
위와 같이 체크박스를 텍스트를 전달하여 생성할 수 있고,
```python
from PySide2.QtWidgets import QCheckBox
from PySide2.QtCore import Qt
widget = QCheckBox("체크박스")
widget.setCheckState(Qt.Checked)
```
시작 할 때 위젯이 체크된 상태로 시작 할 수 도 있다.

### 소스코드1
**wid3.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QCheckBox
from PySide2.QtCore import Qt

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        checkbox = QCheckBox("체크박스")
        checkbox.stateChanged.connect(self.state_changed)

        self.setCentralWidget(checkbox)
        
    def state_changed(self, state):
        print(state, state == Qt.Checked)

app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```
### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid3.gif){: .align-center}

### 살펴보기
```python
class MainWindow(QMainWindow):
    def __init__(self):
        # ...
        checkbox = QCheckBox("체크박스")
        checkbox.stateChanged.connect(self.state_changed)
        
    def state_changed(self, state):
        # ...
```
단계별로 살펴보면  
`QCheckBox` 객체생성  
`state_changed` 매소드(슬롯)생성  
`stateChanged`시그널을 `state_changed`슬롯에 `.connect`연결.  
`state`는 임의로 지정해준 인수이름로 시그널이 발생시킨 이미 정의된(pre-defined) 데이터를 전송하는 역할을
수행하고 있다. 여기선(0 또는 2)

### 소스코드2
**wid4.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QCheckBox
from PySide2.QtCore import Qt

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        checkbox = QCheckBox("3옵션 체크박스")
        checkbox.stateChanged.connect(self.state_changed)
        checkbox.setTristate(True)
        checkbox.setCheckState(Qt.PartiallyChecked)

        self.setCentralWidget(checkbox)
        
    def state_changed(self, state):
        if state == Qt.Checked:
            print(state, "Checked")
        elif state == Qt.Unchecked:
            print(state, "Unchecked")
        elif state == Qt.PartiallyChecked:
            print(state, "PartiallyChecked")

app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid4.gif){: .align-center}  

### 살펴보기
```python
class MainWindow(QMainWindow):
    def __init__(self):
        # ...
        checkbox.setTristate(True)
        checkbox.setCheckState(Qt.PartiallyChecked)
        
    def state_changed(self, state):
        if state == Qt.Checked:
        elif state == Qt.Unchecked:
        elif state == Qt.PartiallyChecked:
```
`setTristate`는 3옵션 체크박스를 활성화 시키는 것이고,  
`setCheckState`는 처음 위젯의 상태를 결정하는 것으로 여기서는  
`Qt.PartiallyChecked`를 지정해서 예와 아니오의 중간단계로 시작.  
`state_changed`에서는 위에서와 마찬가지로 `state`라는 데이터를 수신하여 현재 위젯의
상태가 어떤상탠지 검사하는 매소드를 구현 하였다.

**Check State Flags**  
@ `Qt.Unchecked`: 체크 되지 않은 상태로, 데이터는 0 을 송신한다.  
@ `Qt.PartiallyChecked`: 중간 상태로, 데이터는 1을 송신한다.  
@ `Qt.Checked`: 체크가 된 상태로, 데이터는 2를 송신한다.  
{: .notice--info}

## 4. QComboBox
### 소스코드
**wid5.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QComboBox, QMainWindow

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        widget = QComboBox()

        widget.addItems(["First", "Second", "Third"])
        widget.currentIndexChanged.connect(self.index_changed)
        widget.currentTextChanged.connect(self.text_changed)

        self.setCentralWidget(widget)

    def index_changed(self, data):
        print('index_changed', data)

    def text_changed(self, data):
        print('item_changed', data)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid5.gif){: .align-center}

## 5. QListBox
### 소스코드
**wid6.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QListWidget

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        widget = QListWidget()
        
        widget.addItems(["First", "Second", "Third"])
        widget.currentItemChanged.connect(self.item_changed)
        widget.currentTextChanged.connect(self.text_changed)
        
        self.setCentralWidget(widget)

    def item_changed(self, data):
        print('item_changed', data.text())

    def text_changed(self, data):
        print('text_changed', data)
        
app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid6.gif){: .align-center}

## 6. QLineEdit
### 소스코드
**wid7.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QLineEdit

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        widget = QLineEdit()

        widget.setPlaceholderText("place_holder_text")
        widget.returnPressed.connect(self.return_pressed)
        widget.selectionChanged.connect(self.selection_changed)
        widget.textChanged.connect(self.text_changed)
        widget.textEdited.connect(self.text_edited)
        
        self.setCentralWidget(widget)

    def return_pressed(self):
        self.centralWidget().setText("return_pressed")
        print("[return_pressed]")

    def selection_changed(self):
        s = self.centralWidget().selectedText()
        print("[selection_changed]", data)
        
    def text_changed(self, data):
        print("[text_changed]", data)

    def text_edited(self, data):
        print("[text_edited]", data)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid7.gif){: .align-center}

## 7. QSpinBox, QDoubleSpinBox
### 소스코드
**wid8.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QSpinBox

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        widget = QSpinBox()
        # widget = QDoubleSpinBox()
        
        widget.setMinimum(-10)
        widget.setMaximum(3)
        # widget.setRange(-10,3)

        widget.setPrefix("$")
        widget.setSuffix("c")
        widget.setSingleStep(3) # 0.5 for QDoubleSpinBox
        widget.valueChanged.connect(self.value_changed)
        widget.valueChanged[str].connect(self.value_changed_str)
        
        self.setCentralWidget(widget)
    
    def value_changed(self, data):
        print(data)
    
    def value_changed_str(self, data):
        print(data)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid8.gif){: .align-center}

## 8. QSlider
### 소스코드
**wid9.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QSlider

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        widget = QSlider()
        widget.setMinimum(-10)
        widget.setMaximum(3)
        # Or: widget.setRange(-10,3)
        
        widget.setSingleStep(3)
        widget.valueChanged.connect(self.value_changed)
        widget.sliderMoved.connect(self.slider_position)
        widget.sliderPressed.connect(self.slider_pressed)
        widget.sliderReleased.connect(self.slider_released)
        
        self.setCentralWidget(widget)
    
    def value_changed(self, data):
        print(data)
    
    def slider_position(self, data):
        print("position", data)
    
    def slider_pressed(self):
        print("Pressed!")
    
    def slider_released(self):
        print("Released")

app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid9.gif){: .align-center}

## 9. QDial
### 소스코드
**wid10.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QDial

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("My App")
        widget = QDial()
        widget.setRange(-10, 100)
        widget.setSingleStep(0.5)
        widget.valueChanged.connect(self.value_changed)
        widget.sliderMoved.connect(self.slider_position)
        widget.sliderPressed.connect(self.slider_pressed)
        widget.sliderReleased.connect(self.slider_released)
        self.setCentralWidget(widget)
        
    def value_changed(self, i):
        print(i)

    def slider_position(self, p):
        print("position", p)

    def slider_pressed(self):
        print("Pressed!")

    def slider_released(self):
        print("Released")

app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid10.gif){: .align-center}

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2 [Official Link](www.learnpyqt.com){: .btn .btn--inverse}
