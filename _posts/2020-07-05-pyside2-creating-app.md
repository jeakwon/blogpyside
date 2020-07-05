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

이번 포스트에서는 데스크톱에 PySide2로 Qt App을 생성해 보겠다.

# Creating Basic Pyside2 Application
## 1. 창만들기
**`main.py`**
```python
import sys
from PySide2.QtWidgets import QApplication, QWdiget

app = QApplication(sys.argv)
win = QWidget
win.show()
app.exec_()
```

# 참고
* This post was written based on Martin Fitzpatrick's Create GUI Applications with QT & Python - PySide2 [Official Link](www.learnpyqt.com){: .btn .btn--inverse}
