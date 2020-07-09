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
[소스코드](https://github.com/jeakwon/pyside2/tree/master/03_widgets){: .btn .btn--primary}

# 1. 위젯이란?
[전편](https://jeakwon.github.io/pyside2/pyside2-signal-and-slot/)에서는 시그널&슬롯의 개념에 대하여 
`QPushButton`위젯을 통해서 알아보았다. Qt에는 `QPushButton`이외에도 다양한 위젯들이 존재한다. 위젯이란
**창 안에 존재하는 사용자가 상호작용할 수 있는 UI 컴포넌트**이다. Qt에서는 많은 종류의 위젯이 존재하고
사용자가 새롭게 만들 수도 있다.

Qt에서 제공하는 위젯의 종류가 정말 많지만, 아래 코드에서는 그 중에서 몇가지 가시적인 것들만
실행해 보았다. 코멘트 처리 된 것을 해제하여 각각의 위젯을 살펴보기 바란다.

## 소스코드
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

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid1.png){: .align-center}  
위 사진에서 볼 수 있듯, Qt에 내장된 위젯은 형태 뿐만 아니라 기능적으로도
파일접근, 날짜, 폰트, 색깔선택 등 상당히 유용하다. 이는 윈도우뿐만 아니라 MacOS, 리눅스에서도 같은 코드로 구동이 가능하다.

## 살펴보기
Qt의 공식적인 위젯 리스트 및 설명을 보기 위해서는 [Official Link](https://doc.qt.io/qt-5/qtwidgets-module.html){: .btn .btn--primary}을 참고하기 바란다.

# 2. QLabel
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

## 소스코드
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

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid2.png){: .align-center}  

## 살펴보기
## 1. MainWindow 위젯
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

## 2. Label 위젯
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


## 3. font
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

# 3. QCheckBox
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

## 소스코드1
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
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid3.gif){: .align-center}

## 살펴보기
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

## 소스코드2
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

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid4.gif){: .align-center}  

## 살펴보기
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

# 4. QComboBox
`QComboBox`는 드롭다운 리스트로, 화살표를 눌러 리스트에서 아이템을 선택한다. 그리고 현재 
선택되어진 아이템이 위젯에 보여지게 된다. 많은 선택지 중 하나를 고를 때 유용한 위젯이다.
대표적으로 많이 사용되는 예가 글자 폰트의 종류나 사이즈를 선택할때 많이 사용된다. 사실
Qt는 폰트 선택 옵션을 이미 `QFontComboBox` 위젯으로 제공하고 있다.

## 소스코드
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

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid5.gif){: .align-center}

## 설명
`.currentIndexChanged`는 콤보박스의 시그널로, 선택된 아이템이 변화시 변화된 `index`를 `int`로 보낸다
`.currentTextChanged`역시 콤보박스의 시그널로, 선택된 아이템이 변화시 변화된 `text`를 `str`로 보낸다

**유저가 직접 아이템에 추가하게 하려면**  
@ 콤보박스는 수정이 가능한데, `.setEditable(True)` 유저가 실제로 값을 입력하여 리스트에 포함 시킬 수 있다.  
@ 추가할때 리스트 업데이트 방식을 `.setInsertPolicy(QComboBox.InsertAtTop)`로 설정해줄 수 있다.  
@ 최대 개수는 `.setMaxCount(20)`로 제한 할 수도 있다.
{: .notice--primary}

**QComboBox Insert Behavior Flags**  
@ QComboBox.NoInsert  
@ QComboBox.InsertAtTop  
@ QComboBox.InsertAtCurrent  
@ QComboBox.InsertAtBottom  
@ QComboBox.InsertAfterCurrent  
@ QComboBox.InsertBeforeCurrent  
@ QComboBox.InsertAlphabetically  
{: .notice--info}


# 5. QListBox
`QListBox`는 `QComboBox`와 유사하지만, 하나의 아이템만 표시하는 것이 아니라, 리스트 자체를 스크롤 가능한
형태로 보여주고, 한번에 여러개 선택을 가능하게 해준다. `currentTextChanged`를 이용하면 현재 아이템의 Text
시그널을 보내주지만, `currentItemChanged` 시그널이용하면 `QListItem`형태로 객체 자체를 시그널로 전송해 줄
수도 있다. 

## 소스코드
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

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid6.gif){: .align-center}

## 살펴보기
앞서서 설명했지만, `currentItemChanged`를 보면
```python
    def item_changed(self, data):
        print('item_changed', data.text())
```
`data`를 아이템 형태로 받기 때문에, 아이템 자체를 조작하거나, `.text()`로 텍스트에 접근할 수 있다.

# 6. QLineEdit
`QLineEdit`위젯은 단순한 한줄 텍스트의 수정가능한 박스로, 회원가입등을 할때 아이디/비밀번호/이메일/성명 
등을 입력시 많이 접해본 형태의 위젯일 것이다. 이 필드를 이용해서 유저의 입력을 날것으로 받아 들일 수도 
있겠지만, 특정형태를 강제하고 싶은 경우, 예를들어 비밀번호 규칙이나, 전화번호, 이메일의 형식의 규격을 
맞추도록 하기 위해서 우리는 그 필드의 값에 접근 할 방법을 알아야 할 것이다.

## 소스코드
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
        data = self.centralWidget().selectedText()
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

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid7.gif){: .align-center}

## 살펴보기
`.setPlaceHolderText`를 이용하면 현재의 위젯이 받고 싶은 정보를 위젯에 표현 할 수 있다. 또한 만약 텍스트
길이 제한을 두고 싶다면 `.setMaxLength(20)`와 같이 설정해 주면 된다. 입력/수정을 막고 읽기만 가능하게 하고
싶다면 `.setReadOnly(True)`를 설정해 주면 된다.

`returnPressed`의 경우, `return`(엔터키)이 눌렸을 때 발동되는 시그널이다.  
`selectionChanged`의 경우 현재 위젯에 입력된 텍스트를 드래그 할때 발생되는 시그널이며,  
`.centralWidget().selectedText()`으로 선택된 텍스트에 접근 할 수 있다.  
`textEdited`와 `textChanged`는 우리가 타이핑을 하거나 지울때 발동되어서 같은 기능으로 착각 할 수 있지만,  
`textEdited`는 능동적으로 텍스트를 타이핑하거나 지웠을 때 발동되는 것이고,  
`textChanged`는 수동적으로 텍스트가 수정되었을 때 발동 되는 것이다.  
차이를 이해하려면 결과에서 `returnPressed`가 발동되어 텍스트가 수정되었을 때, 우리는 능동적으로 수정하지는
않았지만, 텍스트는 변화하였다. 따라서 `textChanged`만 발동이 되게 된다.

**Input Validation example**  
`.setInputMask('000.000.000.000;_')`처럼 입력해주면 IP주소 형식으로 받을 수 있다. 아래는 몇 가지 다른 예시.  
@ `000.000.000.000;_` IP주소. 빈자리를 _로 표시  
@ `HH:HH:HH:HH:HH:HH;_` MAC address(네트워크 어댑터(랜카드)의 물리적 주소). 빈자리를 _로 표시  
@ `0000-00-00` ISO 날짜; 빈자리를 스페이스로 표시  
@ `>AAAAA-AAAAA-AAAAA-AAAAA-AAAAA;#` 라이센스 번호; 모두 대문자로, 빈자리는 #로 표시.  
@ 디테일한 형식은 [공식 링크](https://doc.qt.io/qt-5/qlineedit.html#inputMask-prop)참고
{: .notice--info}

# 7. QSpinBox, QDoubleSpinBox
스핀박스는 숫자 인풋을 받는 위젯으로, 양방향 화살표로 증가/감소를 시킬 수 있다. `QSpinBox`는 정수(int)를, `QDoubleSpinBox`는 소수(float)를 지원한다.

## 소스코드
**wid8.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QSpinBox

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        widget = QSpinBox() # QDoubleSpinBox()
        
        widget.setRange(-10,10)

        widget.setPrefix("$")
        widget.setSuffix("c")
        widget.setSingleStep(1) # 0.1
        widget.valueChanged.connect(self.value_changed)
        widget.valueChanged[str].connect(self.value_changed_str)
        
        self.setCentralWidget(widget)
    
    def value_changed(self, data):
        print("[value_changed]", data)
    
    def value_changed_str(self, data):
        print("[value_changed_str]", data)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid8.gif){: .align-center}

## 살펴보기
1. 위 코드에서 주석처리 된 부분을 원래 코드와 바꾸어 주면 float 타입의 스핀박스 위젯을 테스트 해 볼 수 있다.
2. `.setRange`를 이용하면 인풋으로 받을 범위를 한정지을 수 있다. `.setMinimum`과 `.setMaximum`을 사용해도 무방하다.
3. `.setSingleStep`은 단위를 설정해 줄 수 있다.
4. `.setPrefix`는 접두어 `.setSuffix`는 접미어로, 숫자 이외의 표시를 하는데 이용이 된다.
5. `.valueChanged`시그널은 위젯의 인풋이 변하는 경우 값을 데이터로 전송한다
6. 접두어나 접미어가 붙은 text를 그대로 받는 방법은 `.valueChanged[str]`의 형태로 슬롯을 연결해 주면 된다.


# 8. QSlider
슬라이더는 슬라이더-바 위젯을 제공하는데, 기능 자체는 `QDoubleSpinBox`와 비슷하나, 어떠한 값을 보여주기 보다는 어떤 
범위내에서 그 값에 해당하는 곳에 슬라이더가 위치하는 방식이다. 두 극단의 값에서 어떤 중간값을 찾으면서, 매우 세밀한 값을
조작할 필요가 없을때 적합한 위젯이라고 할 수 있다. 예를 들면 소리의 크기를 조절 할 때, 모니터의 밝기를 조절할 때를 예로 
들 수 있다.

## 소스코드
**wid9.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QSlider

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        widget = QSlider()
        widget.setMinimum(-10)
        widget.setMaximum(10)
        
        widget.setSingleStep(2)
        widget.valueChanged.connect(self.value_changed)
        widget.sliderMoved.connect(self.slider_moved)
        widget.sliderPressed.connect(self.slider_pressed)
        widget.sliderReleased.connect(self.slider_released)
        
        self.setCentralWidget(widget)
    
    def value_changed(self, data):
        print("[value_changed]", data)
    
    def slider_moved(self, data):
        print("[slider_moved]", data)
    
    def slider_pressed(self):
        print("[slider_pressed]")
    
    def slider_released(self):
        print("[slider_released]")

app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid9.gif){: .align-center}

## 살펴보기
1. 먼저 범위를 설정할 때는 `.setMinimum`&`.setMaximum`을 이용하거나 `.setRange(최소, 최대)`를 이용하면 된다
2. 움직이는 단위를 설정하는 것은 `.setSingleStep`으로 가능.
3. `valueChanged`와 `sliderMoved`는 슬라이더를 직접 움직이는 경우엔 똑같이 시그널을 발생 시키지만, 그 외의
방식의 경우, 예를 들면 위 결과에서 슬라이더가 없는 부분을 눌러서 변하는 경우에는 `valueChanged`가 발동된다.
4. 슬라이더에 마우스가 눌릴 때와 떨어질 때 `.sliderPressed`와 `.sliderReleased` 시그널이 발동된다.

**QSlider Flags**  
위젯의 디폴트 슬라이더는 세로형태이다. 가로형태로 바꿔주기 위해서 `widget.QSlider(Qt.Horizontal)`를 추가해
주면 된다.  
@ widget.QSlider(Qt.Vertical)  
@ widget.QSlider(Qt.Horizontal)
{: .notice--info}

# 9. QDial
다이얼 위젯은 슬라이더와 매우 유사하지만, 조금더 아날로그적인 감성의 위젯이다. 디자인적으로는 괜찮지만, 사용자
친화적인 부분은 조금 떨어진다. 하지만, 오디오 어플리케이션같은 곳에서 실제다이얼 느낌을 주기위해 사용되는 경우가
있다.

## 소스코드
**wid10.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QDial

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        widget = QDial()
        widget.setRange(-10, 10)
        widget.setSingleStep(0.1)

        widget.valueChanged.connect(self.value_changed)
        widget.sliderMoved.connect(self.slider_moved)
        widget.sliderPressed.connect(self.slider_pressed)
        widget.sliderReleased.connect(self.slider_released)
        
        self.setCentralWidget(widget)
        
    def value_changed(self, data):
        print("[value_changed]", data)

    def slider_moved(self, data):
        print("[slider_moved]", data)

    def slider_pressed(self):
        print("[slider_pressed]")

    def slider_released(self):
        print("[slider_released]")

app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec_()
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/03_widgets/wid10.gif){: .align-center}

## 살펴보기
슬라이더 위젯과 시그널이 전부 같다는 것을 알 수 있다. 위에서 설명한 것 처럼, `valueChanged`와 `sliderMoved`의 미묘한 차이가
여기서도 똑같이 적용되는 것을 볼 수 있다. pressed와 released는 그 객체 자체에 대한 접근 시그널로 보면 되겠다.

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2 [Official Link](www.learnpyqt.com){: .btn .btn--inverse}
