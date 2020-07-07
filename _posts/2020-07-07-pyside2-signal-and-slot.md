---
date: 2020-07-07 22:42:00 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---
PySide2의 시그널&슬롯 작동원리 이해하기
[소스코드](https://github.com/jeakwon/pyside2/tree/master/2_signal_and_slot){: .btn .btn--primary}

# Singal and Slot - QPushButton
[전편](https://jeakwon.github.io/pyside2/pyside2-create-app/)에서 버튼 위젯으로 간단한 앱을 만들어 보았다.
하지만, 껍데기뿐인 버튼으로 아무런 기능이 없는 상태. 아마도 버튼을 넣은 이유는 우리가 버튼을 눌러서 어떤
변화를 만들기 위해서일 것이다. 이러한 것을 가능하게 해주는 것이 Qt에서는 시그널&슬롯의 개념이다.

## 시그널
시그널은 위젯에서 어떤 일이 일어나면 발생되는 알림같은 것이다. 예를들면 버튼을 눌린다거나, 텍스트 인풋에 
키보드 입력이 이루어 진다거나, 마우스 버튼이 눌린다거나 하는 것들이다. 즉 사용자와의 상호작용에 의해 
발생하는 변화인 것이다. 시그널은 변화가 일어났다는 사실을 알리는 것 뿐만 아니라, 어떤 일이 일어나는지도
그 내용을 알려주는 데이터를 보낼 수도 있다.

## 슬롯
슬롯은 Qt가 사용하는 시그널을 받는 수신자이다. 파이썬에서는 어떠한 함수(=매소드)라도 슬롯으로 사용 될 수 있다.
단순하게 시그널과 연결하기만 하면 되고, 만약 데이터를 보내는 경우에는 함수에 수신 받을 수 있는 인수를 두면
데이터까지도 받을 수 있다. 많은 Qt 위젯은 미리 만들어진 슬롯들이 존재하며, 이것들을 직접 연결해 줄 수 있다.

시그널과 슬롯의 개념과 데이터 저장 등을 6단계에 걸쳐서 살펴보려고 한다.

## 1. 시그널 함수 연결
### 소스코드
**sns1.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        btn = QPushButton("Push")
        btn.clicked.connect(self.btn_clicked)

        self.setCentralWidget(btn)

    def btn_clicked(self):
        print("Clicked")

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과 (클릭 세번)
```
Clicked
Clicked
Clicked
```

### 설명
```python
class MainWindow(QMainWindow):
    def __init__(self):
        #...
        btn.clicked.connect(self.btn_clicked)

    def btn_clicked(self):
        print("Clicked")
```
`btn.clicked`는 시그널이고 `btn_clicked` 우리가 정의 해준 슬롯이다. 이를 `.connect` 메소드를 통해 시그널과 슬롯을 연결하여
생성해준 `QPushButton`이 클릭 시그널을 발생할 때 마다 우리가 만들어준 슬롯이 작동하게 된다.


## 2. 데이터 수신
### 소스코드
**sns2.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        btn = QPushButton("Push")
        btn.clicked.connect(self.btn_clicked)

        self.setCentralWidget(btn)

    def btn_clicked(self, checked): # changed from sns1.py
        print("Clicked", checked) # changed from sns1.py

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과 (클릭 세번)
```
Clicked False
Clicked False
Clicked False
```

### 설명
```python
class MainWindow(QMainWindow):
    #...

    def btn_clicked(self, checked): # changed from sns1.py
        print("Clicked", checked) # changed from sns1.py
```
`btn_clicked(self, checked)`는 시그널이 전송한 데이터가 접근할 인수(argument)로 `checked`를 추가해 준 것이다. 꼭 `checked`로 할 필요는 없다.  

결과를 보면, 수신된 데이터 `checked`가 `False`상태로, 현재 버튼이 눌렸을 때, 우리가 버튼의 상태에 영향을 주는 것은 아니다.
우리는 버튼이 눌렸을 때, 위젯이 상태 변화를 일으키도록 할 수 있다. 다음을 보자.

## 3. 위젯의 상태 변환
### 소스코드
**sns3.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        btn = QPushButton("Push")
        btn.clicked.connect(self.btn_clicked)
        btn.setCheckable(True) # added from sns2.py
        self.setCentralWidget(btn)

    def btn_clicked(self, checked):
        print("Clicked", checked)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과 (클릭 세번)
```
Clicked True
Clicked False
Clicked True
```

### 설명
```python
class MainWindow(QMainWindow):
    def __init__(self):
        #...
        btn.setCheckable(True) # added from sns2.py
```

`.setCheckable(True)`는 `btn`의 `checked` 상태를 변화시킬 수 있게 해준다. 디폴트값으로 False로 되어있다.
이렇게 위젯마다, 이미 내장된 어떤 상태변화를 감지하는 기능들이 있다. 이러한 기능들을 활용하면 과거에 이 위젯과
사용자가 어떠한 상호작용을 일으켰는지 확인 할 수 있을 것이다.

## 4. 데이터 저장
### 소스코드
**sns4.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        btn = QPushButton("Push")
        btn.clicked.connect(self.btn_clicked)
        btn.clicked.connect(self.btn_toggled) # added from sns1.py
        btn.setCheckable(True) # added from sns1.py
        self.setCentralWidget(btn)

    def btn_clicked(self):
        print("Clicked")

    def btn_toggled(self, checked): # added from sns1.py
        self.btn_checked = checked # added from sns1.py
        print(self.btn_checked)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과 (클릭 세번)
```
Clicked
True
Clicked
False
Clicked
True
```

### 설명
```python
class MainWindow(QMainWindow):
        #...
    def btn_toggled(self, checked): # added from sns1.py
        self.btn_checked = checked # added from sns1.py
```

`self.btn_checked = checked`를 통해서 MainWindow 객체에 수신된 데이터를 저장할 수 있다. 
이를 통해서 사용자가 원하는 방식으로 수신된 데이터를 후처리 할 수도 있다.

**데이터 저장**  
위젯의 현재 상태를 파이썬의 변수로 저장하는 것이 용이한 경우가 있다. 왜냐하면 원래 객체에 
접근하지 않더라도 저장된 상태에만 접근하여 작업할 수 있게 만들어 주기 대문이다. 필요하다면
`Dictionary` 형태로 관리할 수도 있으니 참고.
{: .notice--info}


## 5. 버튼 릴리즈는 데이터 수신이 불가
### 소스코드
**sns5.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        btn = QPushButton("Push")
        btn.released.connect(self.btn_released) # changed from sns4.py
        btn.released.connect(self.btn_toggled) # changed from sns4.py
        btn.setCheckable(True)
        self.setCentralWidget(btn)

    def btn_released(self): # changed from sns4.py
        print("Released") # changed from sns4.py

    def btn_toggled(self, checked):  # should invoke TypeError
        self.btn_checked = checked
        print(self.btn_checked)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과 (클릭 한번)
```
Released
TypeError: btn_toggled() missing 1 required positional argument: 'checked'
```

### 설명
```python
class MainWindow(QMainWindow):
    def __init__(self):
        # ...
        btn.released.connect(self.btn_released) # changed from sns4.py
        btn.released.connect(self.btn_toggled) # changed from sns4.py

    def btn_released(self): # changed from sns4.py
        print("Released") # changed from sns4.py

    def btn_toggled(self, checked): # should invoke TypeError
        self.btn_checked = checked
        print(self.btn_checked)
```

결과에서 `TypeError`가 발생한 것을 보면 checked라는 인수로 데이터를 수신하려고 한 `btn_toggled` 함수가 에러를
일으켰다는 것을 알 수 있다. 이는 `.released`라는 시그널은 `.clicked`시그널과는 다르게 제공하는 데이터가 없기
때문에 슬롯에서 받을 데이터가 없어서 발생하는 에러이다.

**시그널의 두 가지 형태**  
@ 변화 및 데이터 두 가지가 있는 경우(ex. btn.clicekd): 이 경우에는 인수를 두지 않으면 변화만 감지하고, 인수를 두면 데이터까지 받을 수 있다.  
@ 변화만 있는 경우(ex. btn.released): 이 경우에는 슬롯에 인수를 두면(=수신할 함수에 인수를 두게되면) 에러를 발생시킨다.
{: .notice--warning}

## 6. 객체 상태 접근을 통한 우회
### 소스코드
**sns6.py**
```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.btn = QPushButton("Push")
        self.btn.released.connect(self.btn_released)
        self.btn.setCheckable(True)
        self.setCentralWidget(self.btn)

    def btn_released(self):
        self.btn_checked = self.btn.isChecked()
        print(self.btn_checked)
        

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
```

### 결과 (클릭 세번)
```
True
False
True
```

### 설명
```python
class MainWindow(QMainWindow):
    def __init__(self):
        # ...
        self.btn = QPushButton("Push")

    def btn_released(self):
        print(self.btn.isChecked())
```

앞선 예제들과는 다르게 이번에는 버튼을 `self.btn`으로 `MainWindow`객체에 속하게
만들었다. 그 이유는 바로 `MainWindow`의 객체에 속한 메소드 `btn_released`에서
`self.btn`객체에 속한 `.isChecked()`라는 메소드에 접근하기 위해서 이다.
이렇게 만들면, **sns5.py**에서 `self.btn_checked`로 데이터를 저장한 것과 같은
효과를 낼 수가 있다.

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2 [Official Link](www.learnpyqt.com){: .btn .btn--inverse}
