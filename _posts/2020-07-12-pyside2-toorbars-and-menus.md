---
date: 2020-07-12 23:00:00 -0000
categories: pyside2
tags:
  - python
  - gui
  - qt
toc: true
header:
  overlay_color: "#333"
---
PySide2의 툴바와 메뉴바에 대하여 알아보자
[소스코드](https://github.com/jeakwon/pyside2/tree/master/05_toolbars_and_menus){: .btn .btn--primary}

# 메인윈도우 UI구성하기
메인윈도우, 즉, `QMainWindw`위젯은 기본적으로 메뉴바, 툴바, 상태바 등을 제공한다. Qt에서 메뉴바와 툴바는
`QAction`을 중간에 두어서 깔끔한 UI컨트롤을 할 수 있게 해준다.

# 1. 툴바
툴바는 우리말로는 대게 "도구 모음"이라고 부르는데, 기능이나 명령어를 메뉴별로 묶어서 하나의 막대모양 인터페이스에
배열한 것을 말한다. 요즘엔 모바일 친화적 디자인으로 인해 툴바를 숨겨놓는 미니멀 디자인이 대세.  
Qt툴바는 아이콘, 텍스트 뿐만아니라 Qt의 어떠한 위젯이라도 포함 할 수 있다. 그러나 가장 이상적인 접근법은 아무래도
`QAction`시스템을 활용하여 디스플레이하는 것이다.

## 소스코드
**tnm1.py**
```python
import sys
from PySide2.QtWidgets import QMainWindow, QApplication, QWidget, QToolBar, QAction, QStatusBar
from PySide2.QtGui import QIcon
from PySide2.QtCore import QSize

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        widget = QWidget()
        self.setCentralWidget(widget)

        toolbar = QToolBar("툴바")
        self.addToolBar(toolbar)
        
        action_text = QAction("텍스트", self)
        toolbar.addAction(action_text)
        
        action_icon = QAction(QIcon("icon.png"), "아이콘", self)
        toolbar.addAction(action_icon)
        toolbar.setIconSize(QSize(16, 16))

        statusbar = QStatusBar(self)
        self.setStatusBar(statusbar)
        action_text.setStatusTip("액션_텍스트")
        action_icon.setStatusTip("액션_아이콘")

        action_icon.triggered.connect(self.on_click)
        action_icon.setCheckable(True)

    def on_click(self, s):
        print("클릭", s)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
sys.exit(app.exec_())
```
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/05_toolbars_and_menus/tnm1.gif){: .align-center}

## 설명
- `QToolBar`를 이용해서 툴바를 생성하고, `.addToolBar`를 이용해 `MainWindow`에 포함시켜준다. 이때 생성된 툴바를 마우스 오른쪽 클릭을 통해서 체크를 해제해주면 사라지게 되는데, 불행히도 다시 추가해 줄 수 있는 오른 클릭을 할 공간이 없다. 아예 처음부터 제거할 수 없게 만들거나, 다시 추가할 수 있는 on/off 인터페이스를 구성해야 한다.
- 단순히 텍스트로 구성된 `QAction("텍스트", self)`으로 액션을 생성한 뒤 이를 `.addAction`으로 툴바에 포함시켜 주면 단순한 텍스트 툴바 버튼이 만들어진다.
- 텍스트가 아닌 아이콘 버튼을 만들기 위해서는 `QAction(QIcon(), 텍스트, self)`형식으로 
액션을 구성하면 된다. 아이콘은 png또는 svg 둘다 사용이 가능하다. (이번 소스코드에서 사용한 `icon.png`는 이 블로그 아이콘으로 연습용으로만 사용하길 바란다). 만약 무료 아이콘패키지를 원한다면, (이 포스트 마지막에서도 사용) MIT 라이센스로 매우 인기가 많고 무료인 [feather](https://feathericons.com/) 아이콘을 추천한다. 
- 툴바에서 아이콘 사이즈를 조절하려면 `.setIconSize(QSize(16, 16))`. 이렇게 지정해 주지 않으면 툴바는 아이콘에 상당히 넓은 여백을 주게된다.
- `QStatusBar(self)`를 이용하면 메인윈도우 하단에 상태표시줄이 추가되며, `QAction`에 
`.setStatusTip`로 상태팁을 적어주면 액션에 접근할 때마다 상태표시가 된다.
- 생성한 액션의 `.triggered` 시그널을 `def on_click(self, s):` 슬롯과 연결해 주었다. 이때 `.setCheckable(True)`를 해주면 수신받는 데이터 `s`가 Boolean형태로 check되었는지 아닌지를 전달하게 된다. (`.toggled`신호도 존재하지만 같은 기능을 한다)

**.setToolButtonStyle**  
사용자에게 보여질 툴바의 스타일을 `QToolBar`객체에 `.setToolButtonStyle(<flag>)`로 설정
해 줄수 있다. 아래는 사용가능한 *flags*  
@ `Qt.ToolButtonIconOnly`: 아이콘만  
@ `Qt.ToolButtonTextOnly`: 텍스트만  
@ `Qt.ToolButtonTextBesideIcon`: 아이콘&텍스트(좌우)  
@ `Qt.ToolButtonTextUnderIcon`: 아이콘&텍스트(상하)  
@ `Qt.ToolButtonFollowStyle`: 호스트 데스크톱 스타일 맞춤  
{: .notice--primary}

# 2. 메뉴바
메뉴는 가장 표준적인 유저 인터페이스라고 볼수 있다. 창의 가장 상단에 위치하였고, 이용가능한
대부분의 기능이 메뉴에서 찾아볼 수 있을 것이다. 자주 이용되는 표준 메뉴들이 존재하는데, 예를 들면 *File*, *Edit*, *View*, *Help*와 같은 메뉴들이다. 또한 메뉴는 드롭다운형태로 계층적인 트리구조를 제공함으로써 체계적으로 구성할 수 있으며, 동시에 단축기등을 지정하여 유저가 원하는 기능에 빠르게 접근 가능하게 한다.

## 소스코드
**tnm2.py**
```python
import sys
from PySide2.QtWidgets import QMainWindow, QApplication, QWidget, QAction, QStatusBar
from PySide2.QtGui import QIcon
from PySide2.QtCore import QSize

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        widget = QWidget()
        self.setCentralWidget(widget)
        
        menubar = self.menuBar()
        menu = menubar.addMenu("&Menu") # "&"" allows short cut access "alt+M"

        action_text = QAction("텍스트", self)
        menu.addAction(action_text)

        action_text.setCheckable(True)

        menu.addSeparator()
        submenu = menu.addMenu("submenu")

        action_icon = QAction(QIcon("icon.png"), "아이콘", self)
        submenu.addAction(action_icon)
        
        action_icon.triggered.connect(self.on_click)
        action_icon.setCheckable(True)
        
        self.setStatusBar(QStatusBar(self))
        action_text.setStatusTip("액션_텍스트")
        action_icon.setStatusTip("액션_아이콘")

    def on_click(self, s):
        print("클릭", s)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
sys.exit(app.exec_())
```
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/05_toolbars_and_menus/tnm2.gif){: .align-center}

## 설명
- `QMainWindow`클래스에서 `.menuBar()`를 통해 이미 내장된 메뉴바 객체를 불러 올 수 있다.
- 불러온 메뉴바에 `.addMenu`를 통해서 메뉴를 추가 할 수 있는데, `"&Menu"`와 같이 **&(ampersand)**를 앞에 붙여주면 alt를 눌렀을때 빠르게 접근 가능한 단축 알파벳을 제공한다.
- 메뉴바 또는 메뉴 객체는 `.addMenu`와 `.addAction`로 서브메뉴를 추가해주거나, 액션을 추가해 줄 수 있다.
- 액션에 `.setCheckable(True)`를 적용해주면, 클릭시에 텍스트 앞에 체크표시(*V*)가 된다.
- `.addSeparator()`를 이용하면 물리적인 구분자를 집어 넣어줄 수 있다.
- 메뉴에도 아이콘액션을 추가할 수 있다. 
- `.triggered`시그널을 이용해 슬롯과 연결 할 수 있지만, 기본적으로 발생되는 시그널은 
`False`이다. 
- 하지만  `.setCheckable(True)`를 해주주면 시그널 발생시 `True`/`False`값이 번갈아가며
전달된다.
- 여기서 알 수 있는 점은 아이콘액션의 경우 클릭시에 앞에 체크표시(*V*)가 없다.
- 툴바와 똑같이, `.setStatusTip`을 통해 메뉴가 아닌 액션에 마우스가 올라가면 상태표시줄에서 상태팁을 표시 가능하다.

**단축키(Short Cut)**  
액션에는 `.setShortcut`로 단축키를 넣어줄 수 있다. 위에서 말한 텍스트 앞에 **&(ampersand)**를 사용하는 것 과는 다르게 `QtGui`>`QKeySequence`를 이용하여 직접 키 시퀀스를 정해 줄 수 있다. 사용방법은 세 가지 방식이 있다.  
@ 스탠다드 숏컷 : 플랫폼 특화된 내장된 키. `QKeySequence(QKeySequence.Print)`  
@ 커스텀 숏컷 : 인간이 읽을 수 있는 string. `QKeySequence("Ctrl+p")`  
@ 하드코딩 숏컷 : Qt에서 정의한 키값. `QKeySequence(Qt.CTRL + Qt.Key_P)`
{: .notice--info}


# 3. 툴바/메뉴바 구성
Qt에서 툴바와 메뉴바는 사용자에게 `QAction`을 통해 기능을 제공하는 인터페이스의 역할을 한다.
만약 유저가 제대로 액션을 찾지 못한다면, 어플리케이션을 제대로 사용할 수 없을 것이다. 따라서,
유저친화적인 UI를 구성해야만, 앱의 기능을 100% 활용할 수 있을 것이다. 마냥 모든 액션들을 툴바에
담는다고해서, 유저가 모든 기능의 진가를 발휘하기를 기대하기는 어렵다. 따라서 필요한 기능끼리 적절하게
적재적소에 담아놓고, 중요도가 높은 액션을 위주로 툴바에 담아내는 것이 바람직한 UI구성일 것이다.
여기서는 File과 Contact라는 가상의 메뉴를 구성하여 그룹핑하고, 무료로 제공되는 아이콘을 활용하여
툴바와 메뉴바를 구성한 앱을 만들었다.

## 소스코드
**tnm3.py**
```python
import sys
from PySide2.QtWidgets import QMainWindow, QApplication, QWidget, QToolBar, QAction, QStatusBar
from PySide2.QtGui import QIcon
from PySide2.QtCore import Qt

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        widget = QWidget()
        self.setCentralWidget(widget)
        
        file_icons = {
            "file" : "icons/file.svg",
            "folder" : "icons/folder.svg",
            "save" : "icons/save.svg",
            "search" : "icons/search.svg",
        }
        contact_icons = {
            "facebook" : "icons/facebook.svg",
            "instagram" : "icons/instagram.svg",
            "twitter" : "icons/twitter.svg",
            "mail" : "icons/mail.svg",
            "phone" : "icons/phone.svg",
        }

        file_actions    = [QAction(QIcon(src), label, self) for label, src in file_icons.items()]
        contact_actions = [QAction(QIcon(src), label, self) for label, src in contact_icons.items()]
        
        menubar = self.menuBar()
        file_menu = menubar.addMenu("File")
        file_menu.addActions(file_actions)

        contact_menu = menubar.addMenu("Contact")
        contact_menu.addActions(contact_actions)
        
        file_toolbar = QToolBar("File")
        file_toolbar.addActions(file_actions)
        self.addToolBar(file_toolbar)

        contact_toolbar = QToolBar("Contact")
        contact_toolbar.addActions(contact_actions)
        self.addToolBar(contact_toolbar)

        self.insertToolBarBreak(contact_toolbar)
        
        # self.addToolBar(Qt.RightToolBarArea, file_toolbar)
        # self.addToolBar(Qt.RightToolBarArea, contact_toolbar)

app = QApplication(sys.argv)
win = MainWindow()
win.show()
sys.exit(app.exec_())
```
## 결과
![](https://raw.githubusercontent.com/jeakwon/pyside2/master/05_toolbars_and_menus/tnm3.gif){: .align-center}

## 설명
- 먼저 [feather](https://feathericons.com/)사이트에서 무료제공되는 아이콘 svg파일을 다운받고 icons라는 폴더에 사용할 9가지의 아이콘을 담았다.
- **File**그룹과 **Contact**그룹으로 나누어 (key : value = label : src) 형태로 dict를 구성하였다.
- 파이썬 *for loop*을 활용해 아이콘 `QAction`을 만들어주었다.
- 메뉴바를 불러와서 `File`메뉴를 추가해주고, File 그룹의 액션들을 `.addActions`으로 한번에 추가해 주었다.
- 메뉴바를 불러와서 `Contact`메뉴를 추가해주고, File 그룹의 액션들을 `.addActions`으로 한번에 추가해 주었다.
- 툴바를 생성한 뒤, `File`툴바를 추가해주고, File 그룹의 액션들을 `.addActions`으로 한번에 추가해 주었다.
- 툴바를 생성한 뒤, `Contact`툴바를 추가해주고, File 그룹의 액션들을 `.addActions`으로 한번에 추가해 주었다.
- `self.insertToolBarBreak(contact_toolbar)`를 이용해 툴바가 서로 다른 행을 쓰도록 분리해 주었다.
- `Qt.RightToolBarArea` Flag를 이용해 툴바의 위치를 오른쪽 세로로 지정해 주었다.

**ToolBarArea Flags** 
`QMainWindow().addToolBar(<flag>, QToolBar())`형태로 *flag*를 전달  
@ `Qt.TopToolBarArea` : 상단에 툴바를 추가한다 (디폴트)  
@ `Qt.BottomToolBarArea` : 하단에 툴바를 추가한다  
@ `Qt.LeftToolBarArea` : 왼쪽에 툴바를 추가한다  
@ `Qt.RightToolBarArea` : 오른쪽에 툴바를 추가한다
{: .notice--info}

**UI구성시 참고사항**  
O 메뉴를 논리적 계층구조로 구성하기  
O 가장 일반적인 기능을 툴바에 넣기  
O 툴바역시 논리적 구조로 그루핑하기  
O 사용될 수 없을 때는 *disable*시키기  
X 같은 액션을 여러 메뉴에 추가하지 않기  
X 모든 메뉴를 툴바에 넣지 않기  
X 같은 액션을 서로 다른 위치와 이름을 사용하지 않기  
X 메뉴에서 아이템을 제거하기보다는 *disable*시키기
{: .notice--warning}

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2  
[Official Link](https://www.learnpyqt.com/){: .btn .btn--inverse}
