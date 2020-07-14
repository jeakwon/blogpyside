---
date: 2020-07-14 01:00:00 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---
PySide2의 다이얼로그 (대화상자)에 대해서 알아보자
[소스코드](https://github.com/jeakwon/pyside2/tree/master/06_dialogs){: .btn .btn--primary}

# 대화상자(Dialog)란?
대화상자는 유용한 GUI요소로써 어플리케이션이 유저와 커뮤니케이션하는 수단이다.
파일 열기, 저장, 설정 등과 같은 기능에 일반적으로 사용된다. 작은 [모달][1] 윈도우 형태로
사용자에게 프로그램 구동에 원하는 커뮤니케이션을 유도하는데 이용된다. Qt는 널리 사용되는 범용성 
대화상자를 많이 제공하고, 좀더 사용자 친화적인 플랫폼 맞춤형태로 제공된다.

[1]: (Modal) 사용자 입력을 독점하는, 자식 윈도에서 부모 윈도로 돌아가기 전에 사용자의 상호동작을 요구하는 창.

# 1. QDialog - 기본구성
## 소스코드
**dlg1.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QDialog, QLabel, QVBoxLayout, QDialogButtonBox)
from PySide2.QtCore import Qt

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        btn = QPushButton("Dialog")
        btn.clicked.connect(self.on_click)
        self.setCentralWidget(btn)
    
    def on_click(self):
        dialog = QDialog(self)
        dialog.setWindowTitle("Dialog")
        
        layout = QVBoxLayout()
        dialog.setLayout(layout)

        label = QLabel("Message")
        label.setAlignment(Qt.AlignCenter)
        layout.addWidget(label)

        btns = QDialogButtonBox.Ok|QDialogButtonBox.Cancel
        btnbox = QDialogButtonBox(btns)
        layout.addWidget(btnbox)

        dialog.exec_()

if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = MainWindow()
    win.show()
    sys.exit(app.exec_())
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg1.gif){: .align-center}

## 설명
- 먼저 `QDialog` Widget을 생성하면 깡통 대화상자가 등장한다. 이 때 메인윈도우를 등지고 있는데, 
대화상자가 생성되면 부모 위젯과는 소통이 불가능해지는 모달 상태가 된다. 메인윈도우에 타이틀을 
붙여준 것 처럼 `setWindowTitle`을 통해 타이틀을 부여해 줄 수 있다.
- QApplication처럼 `.exec_()`를 사용하여 새로운 이벤트루프로 진입하고 창을 띄운다.
- 대화상자역시 여러개의 위젯을 담으려면 `.setLayout`으로 layout을 등록하고, 추가하고 싶은 위젯을 
(여기선 `QLabel`, `QDialogButtonBox`) 담아주면 된다.
- `QDialogButtonBox`에 담을 수 있는 버튼의 종류는 여러가지가 이미 만들어져 있다.파이프(`|`)로 
구분해주면 원하는 버튼을 여러개 담을 수 있다.
- `QDialogButtonBox.Ok`와 `QDialogButtonBox.Cancel`를 박스에 추가해 주어, Ok버튼과 Cancel버튼을 
추가하였으나 아직은 사용자와의 커뮤니케이션이 불가능한 상태.(다음으로)

**Buttons for QDialogButtonBox**  
아래는 사용가능한 버튼의 종류와 버튼이 정의하는 역할  
@ `QDialogButtonBox.Ok` AcceptRole
@ `QDialogButtonBox.Open` AcceptRole
@ `QDialogButtonBox.Save` AcceptRole
@ `QDialogButtonBox.Cancel` RejectRole
@ `QDialogButtonBox.Close` RejectRole
@ `QDialogButtonBox.Discard` DestructiveRole
@ `QDialogButtonBox.Apply` ApplyRole
@ `QDialogButtonBox.Reset` ResetRole
@ `QDialogButtonBox.RestoreDefaults` ResetRole
@ `QDialogButtonBox.Help` HelpRole
@ `QDialogButtonBox.SaveAll` AcceptRole
@ `QDialogButtonBox.Yes` YesRole
@ `QDialogButtonBox.YesToAll` YesRole
@ `QDialogButtonBox.No` NoRole
@ `QDialogButtonBox.NoToAll` NoRole
@ `QDialogButtonBox.Abort` RejectRole
@ `QDialogButtonBox.Retry` AcceptRole
@ `QDialogButtonBox.Ignore` AcceptRole
@ `QDialogButtonBox.NoButton` Invalid Button
{: .notice--info}

**QPushButton대신 QDialogButtonBox을 사용하는 이유**  
`QPushButton`을 레이아웃에 넣어도 충분히 기능 구현은 가능하지만, QDialogButtonBox가 제공하는 
방식은 사용자(호스트데스크톱)의 작업환경 표준조건을 따르기 위함이다. 가령, 버튼의 위치가 좌측인지 
오른쪽인지와 같은 것을 들 수 있다. 따라서 Qt에서는 QDialogButtonBox를 이용하여 커스터마이징을 
하는 것을 권장하고 있다.
{: .notice--warning}

# 2. QDialog - 커스터마이징
`QDialog`의 서브클래스로 `Dialog`라는 커스텀 대화상자를 만들어 본다.
## 소스코드
**dlg2.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QDialog, QLabel, QVBoxLayout, QDialogButtonBox)
from PySide2.QtCore import Qt

class Dialog(QDialog):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Dialog")
        
        layout = QVBoxLayout()

        label = QLabel("Message")
        label.setAlignment(Qt.AlignCenter)
        layout.addWidget(label)
        
        btns = QDialogButtonBox.Ok|QDialogButtonBox.Cancel
        btnbox = QDialogButtonBox(btns)
        layout.addWidget(btnbox)
        
        self.setLayout(layout)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        btn = QPushButton("Dialog")
        btn.clicked.connect(self.on_click)
        self.setCentralWidget(btn)
    
    def on_click(self):
        dialog = Dialog(self)
        dialog.exec_()

if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = MainWindow()
    win.show()
    sys.exit(app.exec_())
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg2.gif){: .align-center}

## 설명
- 최종적으로 모습은 **dlg1.py**와 똑같지만 `QDialog`를 상속받은 독립적인 `Dialog`
서브클래스를 생성하여 반복적인 사용이 가능한 형태가 되었다.
- `QLabel`위젯을 만들어서 `Qt.AlignCenter`를 통해 텍스트를 중앙에 위치 시키고, 
레이아웃에 추가해준다.

# 3. QDialog - 시그널 & 슬롯
## 소스코드
**dlg3.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QDialog, QLabel, QVBoxLayout, QDialogButtonBox)
from PySide2.QtCore import Qt

class Dialog(QDialog):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Dialog")
        
        layout = QVBoxLayout()

        label = QLabel("Message")
        label.setAlignment(Qt.AlignCenter)
        layout.addWidget(label)
        
        btns = QDialogButtonBox.Ok|QDialogButtonBox.Cancel
        btnbox = QDialogButtonBox(btns)
        btnbox.accepted.connect(self.accept)
        btnbox.rejected.connect(self.reject)
        layout.addWidget(btnbox)
        
        self.setLayout(layout)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        btn = QPushButton("Dialog")
        btn.clicked.connect(self.on_click)
        self.setCentralWidget(btn)
    
    def on_click(self):
        dialog = Dialog(self)
        print("Ok" if dialog.exec_() else "Cancel")
            
if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = MainWindow()
    win.show()
    sys.exit(app.exec_())
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg3.gif){: .align-center}

## 설명
```python
        btns = QDialogButtonBox.Ok|QDialogButtonBox.Cancel
        btnbox = QDialogButtonBox(btns)
        btnbox.accepted.connect(self.accept)
        btnbox.rejected.connect(self.reject)
```
- `QDialogButtonBox`클래스의 `.accepted` 시그널을 `QDialogButtonBox.Ok`가 발생시키는 
`AcceptRole`과 연결시키고, `QDialogButtonBox.Cancel`가 발생시키는 `RejectRole`과 
연결시킨다. 
- `QDialog`는 `AcceptRole`과 `RejectRole`이 발생되면 `.exec_()`으로 이벤트 루프를 돌던 
모달 창이 종료되면서 0 또는 1을 반환한다. 이를 이용해 성공적으로 루프가 종료되었는지 print문 
안에 한 줄로 구현하면 아래와 같다. ~~*Pythonic*~~
```python
print("Ok" if dialog.exec_() else "Cancel")
```

# 4. QMessageBox - 기본구성
위에서 대화상자의 기본 구성 패턴을 보았다. 메세지와 버튼. 버튼은 수락 또는 거절.
물론 이러한 대화상자를 스스로 구성할 수도 있지만, Qt는 내장형 대화상자 클래스로 
`QMessageBox`를 제공한다.
## 소스코드
**dlg4.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QMessageBox)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        btn = QPushButton("Dialog")
        btn.clicked.connect(self.on_click)
        self.setCentralWidget(btn)
    
    def on_click(self):
        dialog = QMessageBox(self)
        dialog.setWindowTitle("Question")
        dialog.setText("Do you have any question?")
        dialog.setStandardButtons(QMessageBox.Yes | QMessageBox.No)
        dialog.setIcon(QMessageBox.Question)
        ret = dialog.exec_()
        print("ret:", ret)
        if ret == QMessageBox.Yes:
            print("Yes")
        elif ret == QMessageBox.No:
            print("No")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = MainWindow()
    win.show()
    sys.exit(app.exec_())
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg4.gif){: .align-center}

## 설명
- 먼저 `QMessageBox`로 대화상자를 만든다.
- `.setWindowTitle`로 메인 윈도우의 타이틀을 지정했던것과 똑같이 대화상자 타이틀을 정할 수 있다.
- `.setText`로 메세지를 전달할 수 있다.
- `.setStandardButtons`로 버튼을 구성 한다.
- `.setIcon`로 아이콘을 지정해 줄 수 있다. 
- `QMessageBox`의 이벤트루프 탈출은 **0과 1이 아니라 사용자가 상호작용한 버튼에 해당하는 고유 번호이다**
- 그래서 0과 1로 검사하는 것이 아니라 아래 코드형태로 
```python
        if ret == QMessageBox.Yes:
            print("Yes")
        elif ret == QMessageBox.No:
            print("No")
```
버튼 고유 번호를 비교하는 방식을 따른다.

# 5. QMessageBox - 내장위젯
`QMessageBox`를 이용하면, `QDialog`를 이용하는 것 보다도 더 간편하게 UI를 구성할 수 있다. 
하지만 대화상자를 자세히 들여다보면 특정 패턴이 있는데, 타이틀-아이콘-메세지-버튼으로
이어지는 패턴이다. Qt는 이러한 반복적인 패턴의 메세지박스 위젯을 미리 내장해 두었다.
여기서는 위에서 커스텀으로 만든 `QMessageBox`의 심플버전을 소개한다.

## 소스코드
**dlg5.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QMessageBox)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        btn = QPushButton("Dialog")
        btn.clicked.connect(self.on_click)
        self.setCentralWidget(btn)
    
    def on_click(self):
        ret = QMessageBox.question(
            self, "Question", "Do you have any question?")
        print(ret)
        if ret == QMessageBox.Yes:
            print("Yes")
        elif ret == QMessageBox.No:
            print("No")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = MainWindow()
    win.show()
    sys.exit(app.exec_())
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg5.gif){: .align-center}

## 설명
- 한줄요약: `QMessageBox.question(self, <타이틀>, <메세지>)` 내장 대화상자를 이용하면 부모 
위젯인 `self`와 타이틀, 메세지만 전달하여 간단하게 만들 수 있다.

# 6. QMessageBox - 여러가지타입들 미리보기
Qt에 미리 만들어진 내장형 대화상자 다섯가지를 소개한다. 소스 코드 자체는 단순히 대화상자를
구경하기 위한 전시용 코드이므로, 구체적인 설명은 생략하여도 무방하다.

**Premade QMessageBox**  
@ 기본: `QMessageBox.about(parent, title, text)`: (아이콘: 없음)  
@ 오류: `QMessageBox.critical(parent, title, text)`: (아이콘: X)  
@ 정보: `QMessageBox.information(parent, title, text)`: (아이콘: i)  
@ 질의: `QMessageBox.question(parent, title, text)`: (아이콘: ?)  
@ 경고: `QMessageBox.warning(parent, title, text)`: (아이콘: !)  
{: .notice--primary}

## 소스코드
**dlg6.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QMessageBox, QVBoxLayout, QWidget)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        layout = QVBoxLayout()

        for type_ in ['about','critical','information','question','warning']:
            btn = QPushButton(type_)
            btn.pressed.connect(
                lambda x=type_: getattr(QMessageBox, x)(self, x, x+"_message"))
            layout.addWidget(btn)

        widget = QWidget()
        widget.setLayout(layout)
        self.setCentralWidget(widget)
    
if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = MainWindow()
    win.show()
    sys.exit(app.exec_())
```

## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg6.gif){: .align-center}

## 설명
```python
for type_ in ['about','critical','information','question','warning']:
    btn = QPushButton(type_)
    btn.pressed.connect(
        lambda x=type_: getattr(QMessageBox, x)(self, x, x+"_message"))
    layout.addWidget(btn)
```
- `['about','critical','information','question','warning']`: 다섯종류 대화상자 리스트
- `QPushButton(type_)`: `for` 루프로 버튼생성 및 버튼텍스트를 대화상자이름으로 지정
- `btn.pressed.connect`로 `.pressed`시그널과 아래 `lambda`함수를 연결. 
- `getattr(QMessageBox, x)`: 만약 `x='about'`이 들어온다면 `getattr(QMessageBox, 'about')`
가 될 텐데, 이는 `QMessageBox.about`과 동일하다.
- 따라서 `getattr(QMessageBox, 'about')(self, 'about', 'about'+"_message")`는 
`QMessageBox.about(self, 'about', 'about'+"_message")`과 동일하다.
- `lambda x=type_`으로 매 루프마다 위와같은 함수를 생성하여 연결해 주는 것.
- (심화)이 방식은 `.pressed`에는 작동하지만 `.clicked`에는 작동하지 않는다. 이에 대해서는 간략하게만 
설명하면, `.pressed`는 시그널을 발생시키지 않지만, `.clicked`는 디폴트로 `Boolean` 시그널을 
발생시키기 때문. ~~*자세한 내용이 궁금하면 따로 문의*~~

# 7. 대화상자와 메모리누수(Memory Leak)
일단 간단하게 요약하면, `QDialog`와 `QMessageBox`은 모달윈도우로써, 대화상자 객체를
생성시 부모 위젯을 전달해야만, 제대로 된 Modality[1]를 제공할 것이다. 그런데 이 부모를 
전달하는 당연한 행위가, 어플리케이션의 메모리누수를 촉발시킨다. 소스코드와 결과를 보고 
이야기를 이어나가보겠다.
## 7-1. 메모리 누수 발생 이유
### 소스코드1
**dlg7.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QMessageBox)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        btn = QPushButton("Push")
        btn.clicked.connect(self.with_parent)
        # btn.clicked.connect(self.without_parent)
        self.setCentralWidget(btn)
    
    # memory leak but safe modality
    def with_parent(self):
        dialog = QMessageBox(self) 
        dialog.exec_()
        del dialog
    
    # No memory leak but unsafe modality
    def without_parent(self):
        dialog = QMessageBox() 
        dialog.exec_()

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
print('\n'.join(repr(w) for w in app.allWidgets()))
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg7.gif){: .align-center}

### 설명
결과를 보면 충격적이게도, `QMessageBox(self)`를 하는 것과 `QMessagebox()`를 하는 것에 메모리 누수에 큰
차이가 발생한다. `with_parent` 결과를 보면, 즉 부모를 전달하는 경우, 메세지박스로 생성된 객체들이 어플리케이션이 끝나도
전혀 삭제되어지지 않는 것을 볼 수 있다. 이 것은 다른말로 대화상자가 많이 발생하는 경우(설령 종료했다 하더라도), 
어플리케이션이 끝날때 까지 메모리를 잡아먹고 있는 것을 알 수 있다. 하지만 `without_parent`결과를 보면, 즉 부모를
전달하지 않는 경우, 대화상자로 만들었던 객체들이 다 정리되어, 메모리누수가 없는 상태이다. 어째서 이런 결과가 발생한
것일까.
이에 대한 해답을 [스택오버플로우](https://stackoverflow.com/questions/37918012/pyqt-give-parent-when-creating-a-widget)
에서 찾을 수 있었다. 이러한 현상이 발생하는 이유는 바로 **PySide2가 C++로 만들어진 Qt의 Wrapper**이기 때문에, 
**Python객체를 생성할 수도 있고**, 아니면 부모 위젯에 귀속된 **C++객체를 생성할 수도 있기 때문**이다. 

- 만약 부모에 귀속된 C++객체를 생성하는 경우에는 디폴트로 생성된 객체를 지워주지 않기 때문에 메모리 누수가 발생하지만, 
부모를 전달했기 때문에 모달윈도우의 역할을 제대로 할 수 있다 (결과를 보면 부모 위젯의 정 가운데에서 대화상자가 생성되는 걸 볼 수 있다.)
- 하지만 부모를 전달하지 않는 경우에는 파이썬으로 생성된 객체이기 때문에, 객체가 삭제되는 경우 자동으로 클린업이 이루어지는 것으로 보인다.
하지만 부모가 전달되지 않아서 모달윈도우로써 제대로 기능하지 못하여 메인 윈도우의 가운데가 아닌 현재 데스크톱의 중앙에서 생성이 된다.
- 스택오버플로우에서는 이를 극복할 수 있는 대안을 제시해 주고 있기 때문에 참고하길 바란다.
- 다행히 내장 위젯들은 부모를 전달하여도 메모리 누수가 없이 동작하는 것으로 보인다. (다음 소스코드2 결과 확인)

## 7-2. 메모리 누수가 없는 내장형 대화상자
### 소스코드
**dlg8.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QMessageBox)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        btn = QPushButton("Push")
        btn.clicked.connect(self.on_click)
        self.setCentralWidget(btn)
    
    # No memory leak, proper modal on premade QMessagebox
    def on_click(self):
        QMessageBox.question(self, "title", "question")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = MainWindow()
    win.show()
    app.exec_()
    print('\n'.join(repr(w) for w in app.allWidgets()))
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg8.gif){: .align-center}

### 설명
결과를 보면, `QMessageBox.question(self, "title", "message")`처럼 내장위젯을 이용하면 메모리 누수없이 동작이
되는 것을 확인 할 수 있다. 이는 `question`뿐만아니라 `about`, `critical`, `information`, `warning` 모두
적용되는 사항이다. 이러한 차이를 만들어 내는 가장 큰 차이점으로 짐작이 가는 것은 아무래도 `.exec_()`의 사용유무.
이를 힌트로 미루어 보면 우리가 생성하는 모달 윈도우는 새로운 event loop로 들어갈때 부모가 전달되면서 부모에 귀속되어 
동작하므로 C++코드로 `.exec()`이 일어날 테고, python에서는 cleanup이 불가능 한 상태가 되는 것이로 추정되며,
부모가 전달되지 않으면, python 에서 `.exec()`이 일어나므로 cleanup이 되는 것으로 추정된다. 

따라서, 근본적인 문제는 바로 C++기반 Qt객체를 파이썬코드로 지우지 못해서 일어나는것이기 때문인데, 그렇다면 파이썬에서
Qt객체를 지워줄 수 있는 방법은 전혀 없는 것일까.

## 7-3. 메모리 누수 없애는법
**파이썬에서 C++사이드 Qt객체를 지우는 방법**  
[공식문서](https://doc.qt.io/qtforpython/PySide2/QtCore/QObject.html#PySide2.QtCore.PySide2.QtCore.QObject.deleteLater)를 참고하면, 
`QObject`를 상속받은 모든 객체가 `.deleteLater()`라는 메소드를 통해서 이벤트 루프에 들어가기전 오브젝트 삭제를 예약 할 수 있다!
{: .notice--success}

### 소스코드
**dlg9.py**
```python
import sys
from PySide2.QtWidgets import (QApplication, QMainWindow, 
QPushButton, QMessageBox)

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        btn = QPushButton("Push")
        btn.clicked.connect(self.with_parent)
        self.setCentralWidget(btn)
    
    def with_parent(self):
        dialog = QMessageBox(self)
        dialog.deleteLater()
        dialog.exec_()

app = QApplication(sys.argv)
win = MainWindow()
win.show()
app.exec_()
print('\n'.join(repr(w) for w in app.allWidgets()))
```

### 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/06_dialogs/dlg8.gif){: .align-center}

### 설명
```python
        dialog = QMessageBox(self)
        dialog.deleteLater()
        dialog.exec_()
```
여기서 핵심은 `.deleteLater()`가 `.exec_()`이전에 나와야 한다는 것이다!. 이벤트 루프에 들어가기 전에 미리
객체의 삭제를 예약하는 것. 이것은 모달윈도우에만 적용되는 사항이 `QObject`를 상속받은 모든 객체에 적용 가능한
것이기 때문에 숙지해 두면, 어플리케이션의 메모리 누수를 관리하는데 유용할 것으로 생각된다.

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2  
[Official Link](https://www.learnpyqt.com/){: .btn .btn--inverse}
