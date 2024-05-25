Qt for Python提供了Qt的官方Python绑定， 这使您能够使用Python编写您的Qt应用程序。 该项目有两个主要组成部分：

- PySide2，以便您可以在Python应用程序中使用Qt5 API
- Shiboken2，一个绑定生成器工具，它可以 用于向Python公开C++项目，以及具有 一些实用函数。



# 安装

安装命令

```powershell
pip install PySide2
```



测试安装

```python
import PySide2.QtCore

print(PySide2.__version__)  # 5.15.2.1
print(PySide2.QtCore.__version__)  # 5.15.2
```





# 第1个QtWidgets应用

```python
import sys
from PySide2.QtWidgets import QApplication, QLabel


if __name__ == "__main__":
    app = QApplication(sys.argv)
    label = QLabel("Hello World!")
    label.show()
    app.exec_()
```

这个简单的Hello World程序详细解释：

1. `import sys`: 
   这行代码导入了Python的内置模块`sys`。在PySide2应用中，通常需要导入`sys`模块以便能够传递命令行参数给`QApplication`，比如应用程序的参数列表。

2. `from PySide2.QtWidgets import QApplication, QLabel`: 
   这行代码从PySide2的`QtWidgets`模块中导入了`QApplication`和`QLabel`类。`QApplication`是所有Qt GUI程序的基础，负责管理应用程序的控制流和主事件循环。`QLabel`是一个用于显示文本或图像的基本控件。

3. `app = QApplication(sys.argv)`: 
   创建了一个`QApplication`实例，并将命令行参数传递给它。`sys.argv`是一个列表，包含了从命令行启动程序时传入的所有参数。这行代码是初始化应用程序并准备进入事件循环的必要步骤。通常，您不需要传递任何参数，因此您可以保持原样，或使用 `app = QApplication([])`。

4. `label = QLabel("Hello World!")`: 
   创建了一个`QLabel`对象，并设置其显示的文本为"Hello World!"。这个标签就是一个简单的控件，可以用来展示文本信息。

5. `label.show()`: 
   调用`show()`方法使创建的标签控件在屏幕上可见。没有这一步，控件虽然创建了，但是不会显示出来。

6. `app.exec_()`: 
   这一行启动了Qt的事件循环。当调用`exec_()`方法后，应用程序开始处理用户输入和窗口系统事件，直到用户关闭所有窗口或者调用`quit()`方法。该方法会阻塞在这里，直到应用程序结束。

综上所述，这个程序创建了一个最基础的图形界面应用程序，其中仅包含一个显示“Hello World!”文本的标签。程序首先初始化了应用程序和控件，然后显示控件，最后进入事件循环等待用户交互。当用户关闭窗口时，应用程序会退出。





# 部件样式

## 使用QSS文本

```python
import sys
from PySide2.QtCore import Qt
from PySide2.QtWidgets import QApplication, QLabel

if __name__ == "__main__":
    app = QApplication()
    w = QLabel("This is a placeholder text")
    w.setAlignment(Qt.AlignCenter)
    w.setStyleSheet(
        """
        background-color: #262626;
        color: #FFFFFF;
        font-family: Titillium;
        font-size: 18px;
        """
    )
    w.show()
    sys.exit(app.exec_())

```





## 使用QSS文件

`style.qss`

```python
QLabel {
    background-color: #262626;
    color: #FFFFFF;
    font-family: Titillium;
    font-size: 18px;
}
```



```python
import sys

from PySide2.QtCore import Qt
from PySide2.QtWidgets import QApplication, QLabel

if __name__ == "__main__":
    app = QApplication()

    w = QLabel("This is a placeholder text")
    w.setAlignment(Qt.AlignCenter)

    with open("style.qss", "r") as f:
        _style = f.read()
        w.setStyleSheet(_style)

    w.show()

    sys.exit(app.exec_())
```



# 主窗口 QMainWindow

QMainWindow 组成部分：

![QMainWindow layout](https://doc.qt.io/qtforpython-5/_images/QMainWindow-layout.png)



```python
import sys
from PySide2.QtGui import QKeySequence
from PySide2.QtWidgets import QMainWindow, QAction, QApplication


class MainWindow(QMainWindow):
    def __init__(self):
        QMainWindow.__init__(self)
        self.setWindowTitle("Eartquakes information")

        # Menu
        self.menu = self.menuBar()
        self.file_menu = self.menu.addMenu("File")

        # Exit QAction
        exit_action = QAction("Exit", self)
        exit_action.setShortcut(QKeySequence.Quit)
        exit_action.triggered.connect(self.close)

        self.file_menu.addAction(exit_action)

        # Status Bar
        self.status = self.statusBar()
        self.status.showMessage("Data loaded and plotted")

        # Window dimensions
        geometry = QApplication.desktop().availableGeometry(self)
        self.setFixedSize(int(geometry.width() * 0.8), int(geometry.height() * 0.7))


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()
```





# 界面实现

## 纯代码

```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow, QLabel, QWidget, QVBoxLayout
from PySide2.QtCore import Qt


class MainWindow(QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()

        # 直接将label作为根组件
        # self.label = QLabel("Hello, UI", self)
        # self.label.setAlignment(Qt.AlignCenter)

        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        layout = QVBoxLayout(central_widget)
        label = QLabel("Hello, UI")
        label.setAlignment(Qt.AlignCenter)
        layout.addWidget(label)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()

```

## 引入uic转换后的py

【main.ui】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ui version="4.0">
 <class>MainWindow</class>
 <widget class="QMainWindow" name="MainWindow">
  <property name="geometry">
   <rect>
    <x>0</x>
    <y>0</y>
    <width>289</width>
    <height>101</height>
   </rect>
  </property>
  <property name="windowTitle">
   <string>MainWindow</string>
  </property>
  <widget class="QWidget" name="centralwidget">
   <layout class="QVBoxLayout" name="verticalLayout">
    <item>
     <widget class="QLabel" name="label">
      <property name="text">
       <string>TextLabel</string>
      </property>
      <property name="alignment">
       <set>Qt::AlignCenter</set>
      </property>
     </widget>
    </item>
   </layout>
  </widget>
  <widget class="QMenuBar" name="menubar">
   <property name="geometry">
    <rect>
     <x>0</x>
     <y>0</y>
     <width>289</width>
     <height>23</height>
    </rect>
   </property>
  </widget>
  <widget class="QStatusBar" name="statusbar"/>
 </widget>
 <resources/>
 <connections/>
</ui>
```



`pyside2-uic` 命令工具进行转换

```python
pyside2-uic main.ui -o main_ui.py
```



uic转换后的py文件【main_ui.py】

```python
# -*- coding: utf-8 -*-

################################################################################
## Form generated from reading UI file 'untitled.ui'
##
## Created by: Qt User Interface Compiler version 5.15.2
##
## WARNING! All changes made in this file will be lost when recompiling UI file!
################################################################################

from PySide2.QtCore import *
from PySide2.QtGui import *
from PySide2.QtWidgets import *


class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        if not MainWindow.objectName():
            MainWindow.setObjectName(u"MainWindow")
        MainWindow.resize(289, 101)
        self.centralwidget = QWidget(MainWindow)
        self.centralwidget.setObjectName(u"centralwidget")
        self.verticalLayout = QVBoxLayout(self.centralwidget)
        self.verticalLayout.setObjectName(u"verticalLayout")
        self.label = QLabel(self.centralwidget)
        self.label.setObjectName(u"label")
        self.label.setAlignment(Qt.AlignCenter)

        self.verticalLayout.addWidget(self.label)

        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QMenuBar(MainWindow)
        self.menubar.setObjectName(u"menubar")
        self.menubar.setGeometry(QRect(0, 0, 289, 23))
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QStatusBar(MainWindow)
        self.statusbar.setObjectName(u"statusbar")
        MainWindow.setStatusBar(self.statusbar)

        self.retranslateUi(MainWindow)

        QMetaObject.connectSlotsByName(MainWindow)
    # setupUi

    def retranslateUi(self, MainWindow):
        MainWindow.setWindowTitle(QCoreApplication.translate("MainWindow", u"MainWindow", None))
        self.label.setText(QCoreApplication.translate("MainWindow", u"TextLabel", None))
    # retranslateUi

```



引入 main_ui.py 使用

```python
import sys
from PySide2.QtWidgets import QApplication, QMainWindow
from PySide2.QtCore import QFile
from main_ui import Ui_MainWindow


class MainWindow(QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()
        self.ui = Ui_MainWindow()
        self.ui.setupUi(self)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())
```



## 直接加载UI文件
这种方式，没有控件方法或属性提示。（不建议使用这种方式）

```python
import sys
from PySide2.QtUiTools import QUiLoader
from PySide2.QtWidgets import QApplication
from PySide2.QtCore import QFile, QIODevice

if __name__ == "__main__":
    app = QApplication(sys.argv)

    ui_file_name = "main.ui"
    ui_file = QFile(ui_file_name)
    if not ui_file.open(QIODevice.ReadOnly):
        print(f"Cannot open {ui_file_name}: {ui_file.errorString()}")
        sys.exit(-1)

    loader = QUiLoader()
    window = loader.load(ui_file)
    ui_file.close()

    if not window:
        print(loader.errorString())
        sys.exit(-1)

    window.show()
    sys.exit(app.exec_())
    
```





# 信号与槽

信号(Signal)和槽(Slot)是Qt中的核心机制，也是在PySide编程中对象之间进行通信的机制。

在Qt中，每一个QObject对象和PySide中所有继承自QWidget的控件(这些都是QObject的子对象)都支持信号与槽机制。

当信号发射时，连接的槽函数将会自动执行。

使用`@Slot`装饰器是可选的，但推荐使用，因为它提供了类型检查和更好的代码可读性。当你省略`@Slot`装饰器时，你的函数仍然可以作为槽函数使用。



## 内置信号与内置槽

点击按钮后，按钮会自动隐藏。

```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton, QWidget, QVBoxLayout


class MainWindow(QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()

        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        layout = QVBoxLayout(central_widget)

        button = QPushButton("点击")
        button.clicked.connect(button.hide)

        layout.addWidget(button)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()

```



## 内置信号与自定义槽

```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton, QWidget, QVBoxLayout
from PySide2.QtCore import Slot


class MainWindow(QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()

        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        layout = QVBoxLayout(central_widget)

        button = QPushButton("点击")
        button.clicked.connect(self.func)

        layout.addWidget(button)

    @Slot()
    def func(self):
        print("按钮被点击")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()

```




## 自定义信号与自定义槽
信号参数

```python
sn = Signal(int)  # Python类型
sn = Signal(QUrl)  # Qt类型
sn = Signal(int, str, int)  # 多参数
```

name参数可以定义别名

```python
sn = Signal(int, name='rangeChanged')
```



步骤：

1. 创建信号
2. 连接信号与槽
3. 创建槽
4. 发送信号


```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow
from PySide2.QtCore import Signal, Slot


class MainWindow(QMainWindow):
    # 信号
    sn_speak = Signal(str, name="sn_speaking")

    def __init__(self):
        super(MainWindow, self).__init__()

        # 连接信号与槽
        self.sn_speak.connect(self.say)

    # 槽函数
    @Slot(str)
    def say(self, msg):
        print(f"say: {msg}")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()

    # 发送信号
    main_window.sn_speak.emit("hello, signal and slot")

    # 使用别名发送信号
    main_window.sn_speaking.emit("hello, signal and slot! 哈哈哈！")

    app.exec_()

```



## 内置槽传递自定义参数

```python
import sys
from functools import partial

from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton, QWidget, QVBoxLayout
from PySide2.QtCore import Slot


class MainWindow(QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()

        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        layout = QVBoxLayout(central_widget)

        button = QPushButton("点击")

        # 方式1：使用lambda表达式传递参数（遍历循环时使用，存在引用问题）
        # button.clicked.connect(lambda: self.func("a"))

        # 方式2：通过functools.partial传递参数
        button.clicked.connect(partial(self.func, "a"))

        layout.addWidget(button)

    @Slot(str)
    def func(self, value):
        print(f"接收值：{value}")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()

```




## 信号连接信号

```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow, QPushButton, QWidget, QVBoxLayout
from PySide2.QtCore import Slot, Signal


class MainWindow(QMainWindow):
    sn = Signal()

    def __init__(self):
        super(MainWindow, self).__init__()

        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        layout = QVBoxLayout(central_widget)

        button = QPushButton("点击")
        button.clicked.connect(self.sn)
        self.sn.connect(self.func)

        layout.addWidget(button)

    @Slot()
    def func(self):
        print("按钮被点击")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()

```

## 一个信号连接多个槽函数
如果多个槽连接到一个信号，当信号发出时，槽将按照它们连接的顺序一个接一个地执行。


```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow
from PySide2.QtCore import Slot, Signal


class MainWindow(QMainWindow):
    sn = Signal()

    def __init__(self):
        super(MainWindow, self).__init__()

        self.sn.connect(self.func)
        self.sn.connect(self.func2)
        self.sn.emit()

    @Slot()
    def func(self):
        print("func")

    @Slot()
    def func2(self):
        print("func2")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()

```


## 多个信号连接一个槽函数
```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow
from PySide2.QtCore import Signal, Slot


class MainWindow(QMainWindow):
    # 信号
    sn_int = Signal(int)
    sn_str = Signal(str)

    def __init__(self):
        super(MainWindow, self).__init__()

        # 连接信号与槽
        self.sn_int.connect(self.func)
        self.sn_str.connect(self.func)

    # 槽函数
    @Slot(int)
    @Slot(str)
    def func(self, value):
        if isinstance(value, int):
            print(f"number: {value}")

        elif isinstance(value, str):
            print(f"string: {value}")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()

    # 发送信号
    main_window.sn_int.emit(1)
    main_window.sn_str.emit("a")

    app.exec_()

```



## 断开连接

```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow
from PySide2.QtCore import Slot, Signal


class MainWindow(QMainWindow):
    sn = Signal()

    def __init__(self):
        super(MainWindow, self).__init__()

        # 连接信号sn与槽函数func
        self.sn.connect(self.func)

        # 发射信号sn，触发与其连接的槽函数func执行
        self.sn.emit()

        # 断开信号sn与槽函数func的连接
        self.sn.disconnect(self.func)

        # 连接信号sn与槽函数func2
        self.sn.connect(self.func2)

        # 再次发射信号sn，此时触发与其连接的槽函数func2执行
        self.sn.emit()

        # 断开信号sn与槽函数func2的连接
        self.sn.disconnect(self.func2)

    # 使用Slot装饰器标记槽函数func
    @Slot()
    def func(self):
        print("func")  # 执行func时输出"func"

    # 使用Slot装饰器标记槽函数func2
    @Slot()
    def func2(self):
        print("func2")  # 执行func2时输出"func2"


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()

```


## 连接信号
当一个信号发出时，与之相连的插槽通常会立即执行，就像正常的函数调用一样。当这种情况发生时，信号和插槽机制完全独立于任何GUI事件循环。一旦所有插槽都返回，就会执行emit语句后面的代码。使用 **queued connections** 时情况略有不同；在这种情况下，emit关键字后面的代码会立即继续，插槽会稍后执行。

```python
# 连接语法
Signal.connect(receiver[, type=Qt.AutoConnection])
```
`Signal.connect()` 方法的 `type` 参数用于指定信号与槽之间的连接类型，它决定了信号是在哪个线程中触发槽。`type` 参数支持以下几种值：

1. `Qt.AutoConnection` （默认值）
   - 如果接收者（槽）与发送者（信号）在同一个线程中，则直接调用槽函数。
   - 如果接收者在不同的线程中，则信号会在接收者的线程的事件循环中排队等待处理，即异步调用。

2. `Qt.DirectConnection`
   - 不管接收者在哪一个线程，都会立即调用槽函数。
   - 如果接收者和发送者不在同一个线程，这种连接方式可能会引发数据竞争和其他并发问题，因此在多线程环境下需要谨慎使用。

3. `Qt.QueuedConnection`
   - 无论接收者和发送者是否在同一线程，信号总是在接收者所在线程的事件循环中排队等待处理。
   - 这种连接方式是线程安全的，适用于多线程环境，尤其当槽函数需要访问接收者线程专属的资源时。

总之，在大多数情况下推荐使用默认的 `Qt.AutoConnection`，它能够根据实际情况自动选择合适的连接方式。而如果你明确知道自己的程序设计并能够处理多线程同步问题时，可以选择 `Qt.DirectConnection` 或 `Qt.QueuedConnection`。





# 多线程任务 QThread

在PySide2中，`QThread`类用于处理多线程任务，它是Qt多线程框架的一部分，允许用户将耗时的任务放在单独的线程中执行，以避免UI界面冻结。



## 创建和使用


### 继承QThread

以下是一个使用`QThread`的基本示例，展示了如何在PySide2中创建和使用线程：

```python
import sys
import time

from PySide2.QtWidgets import QApplication, QMainWindow, QLabel
from PySide2.QtCore import QThread, Signal, Slot
from PySide2.QtGui import QCloseEvent


# 任务线程
class WorkerThread(QThread):
    updateSignal = Signal(str)

    def run(self):
        for i in range(5):
            self.sleep(1)
            self.updateSignal.emit(f"Processing... {i+1}/5")


# 主窗口
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        self.label = QLabel("Starting task...", self)
        self.label.move(50, 50)
        self.setCentralWidget(self.label)

        self.thread = WorkerThread()
        self.thread.updateSignal.connect(self.on_update)
        self.thread.finished.connect(self.on_finished)

    # 开始
    def start_task(self):
        self.thread.start()

    # 更新进度
    @Slot(str)
    def on_update(self, msg: str):
        self.label.setText(msg)

    # 结束
    @Slot()
    def on_finished(self):
        self.label.setText("Task completed.")
        self.close_thread()

    # 关闭窗口
    def closeEvent(self, event: QCloseEvent):
        self.close_thread()
        super().closeEvent(event)

    # 关闭线程
    def close_thread(self):
        if self.thread.isRunning():
            self.thread.quit()
            self.thread.wait()


if __name__ == "__main__":
    app = QApplication(sys.argv)
    mainWindow = MainWindow()
    mainWindow.show()
    mainWindow.start_task()
    sys.exit(app.exec_())

```

在这个示例中，我们定义了一个`WorkerThread`子类，继承自`QThread`，并在其中重写了`run`方法来执行耗时任务。我们还定义了一个自定义信号`updateSignal`，用于在任务执行过程中更新UI。主线程中的`MainWindow`类连接了这个信号到`on_update`槽函数，从而能够在任务执行的不同阶段更新界面上的标签文本。

记住，由于PySide2/Qt是基于C++的，直接在`QThread.run`中修改UI元素是不安全的，因此通常会通过信号-槽机制来安全地跨线程更新UI。





### 使用movetothread

PySide（以及其同源库Qt for Python和Qt本身）推荐避免直接继承`QThread`并重写`run()`方法来实现多线程的原因，主要是出于设计模式和资源管理的考虑。以下是一些主要原因：

1. **对象生命周期管理复杂**：当直接在子类化的`QThread`中运行代码时，线程的生命周期与该对象紧密绑定。这可能导致资源释放问题，比如在线程还在运行时对象就被意外销毁了。

2. **耦合度高**：将业务逻辑直接嵌入到线程类中，会使得业务逻辑与线程管理逻辑高度耦合，不利于代码的维护和重用。

3. **信号与槽的限制**：虽然Qt提供了强大的信号与槽机制用于线程间通信，但在自定义`QThread`子类中，直接操作UI组件或访问共享数据时，如果不正确使用信号与槽或者锁机制，很容易引发线程安全问题。

4. **官方推荐使用 worker-object 模式**：Qt官方更推荐使用“工作者对象”模式，即创建一个不继承自`QThread`的普通对象（工作者对象），然后将这个对象移到一个单独的线程中执行。这样可以更好地分离线程管理和实际的工作逻辑，提高代码的清晰度和可维护性。

具体做法是：
- 创建一个普通类（工作者类）来封装你的任务逻辑。
- 使用`QThread`实例来管理这个工作者对象的线程上下文。
- 通过信号与槽机制来实现工作者对象与主线程或其他线程之间的通信和数据同步。
- 利用`moveToThread()`方法将工作者对象移动到目标线程中执行。

这种做法的优势在于它鼓励更好的面向对象设计原则的应用，比如单一职责原则，同时使得多线程的管理更加集中和清晰，有助于避免潜在的内存泄漏和线程安全问题。


```python
import sys
import time

from PySide2.QtWidgets import QApplication, QMainWindow, QLabel
from PySide2.QtCore import QThread, Signal, Slot, QObject
from PySide2.QtGui import QCloseEvent


# 任务类
class Worker(QObject):
    updateSignal = Signal(str)
    finishedSignal = Signal()

    def process(self):
        for i in range(5):
            time.sleep(1)
            self.updateSignal.emit(f"Processing... {i+1}/5")
        self.finishedSignal.emit()


# 主窗口
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        self.label = QLabel("Starting task...", self)
        self.label.move(50, 50)
        self.setCentralWidget(self.label)

        self.worker = Worker()
        self.workerThread = QThread()

        self.worker.moveToThread(self.workerThread)
        self.worker.updateSignal.connect(self.on_update)
        self.worker.finishedSignal.connect(self.on_task_finished)
        self.workerThread.started.connect(self.worker.process)

    # 开始
    def start_task(self):
        self.workerThread.start()

    # 更新进度
    @Slot(str)
    def on_update(self, msg: str):
        self.label.setText(msg)

    # 结束
    @Slot()
    def on_task_finished(self):
        self.label.setText("Task completed.")
        self.close_thread()

    # 关闭窗口
    def closeEvent(self, event: QCloseEvent):
        self.close_thread()
        super().closeEvent(event)

    # 关闭线程
    def close_thread(self):
        if self.workerThread.isRunning():
            self.workerThread.quit()
            self.workerThread.wait()


if __name__ == "__main__":
    app = QApplication(sys.argv)
    mainWindow = MainWindow()
    mainWindow.show()
    mainWindow.start_task()
    sys.exit(app.exec_())

```


## 暂停、继续、终止

程序实现了一个能够显示0-99数字循环进度的功能，并提供了进度查看、暂停、继续及终止操作。

```python
import sys
import time

from PySide2.QtCore import QObject, QThread, Qt, Signal, QTimer, QCoreApplication, QEventLoop, Slot
from PySide2.QtWidgets import QMainWindow, QWidget, QVBoxLayout, QTextEdit, QPushButton, QApplication


class Worker(QThread):
    sn_progress = Signal(str)
    sn_paused = Signal()
    sn_resumed = Signal()

    def __init__(self, parent=None):
        super().__init__(parent)

        self.flag_stop = False  # 标志：是否停止任务
        self.flag_pause = False  # 标志：是否暂停任务

    def run(self):
        num = 100

        for i in range(num):
            if self.flag_stop:  # 检查是否需要停止任务的执行
                return

            if self.flag_pause:  # 检查是否需要暂停任务的执行
                self.sn_paused.emit()  # 发射暂停信号
                while self.flag_pause:  # 循环等待直到暂停状态解除
                    time.sleep(0.1)
                    if self.flag_stop:  # 如果需要停止任务，立即返回
                        return
                self.sn_resumed.emit()  # 发射恢复执行信号

            self.sn_progress.emit(str(i))  # 发射进度信息信号
            time.sleep(0.1)  # 延迟0.1秒

    def stop(self):
        self.flag_stop = True  # 设置停止标志

    def pause(self):
        self.flag_pause = True  # 设置暂停标志

    def resume(self):
        self.flag_pause = False  # 清除暂停标志


class MainWindow(QMainWindow):
    def __init__(self, parent=None):
        super().__init__(parent)

        self.setWindowTitle("主窗口")
        self.resize(300, 200)

        self.widget = QWidget()
        layout = QVBoxLayout()
        self.widget.setLayout(layout)
        self.setCentralWidget(self.widget)

        self.text_edit = QTextEdit(self)
        self.btn_start = QPushButton("开始")
        self.btn_stop = QPushButton("终止")
        self.btn_pause = QPushButton("暂停")
        self.btn_resume = QPushButton("继续")

        self.btn_stop.setEnabled(False)
        self.btn_pause.setEnabled(False)
        self.btn_resume.setEnabled(False)

        layout.addWidget(self.text_edit)
        layout.addWidget(self.btn_start)
        layout.addWidget(self.btn_stop)
        layout.addWidget(self.btn_pause)
        layout.addWidget(self.btn_resume)

        # 工作线程对象
        self.worker = None

        # 终止标记。终止后线程也会触发finished信号，导致界面出现结束日志
        self.flag_stop = False

        # 事件
        self.btn_start.clicked.connect(self.start)  # 开始
        self.btn_stop.clicked.connect(self.stop)  # 终止
        self.btn_pause.clicked.connect(self.pause)  # 暂停
        self.btn_resume.clicked.connect(self.resume)  # 继续

    @Slot()
    def start(self):
        self.worker = Worker()  # 创建工作线程
        self.worker.sn_progress.connect(self.handle_sn_progress)  # 处理进度
        self.worker.sn_paused.connect(self.handle_sn_paused)  # 处理暂停
        self.worker.sn_resumed.connect(self.handle_sn_resumed)  # 处理继续
        self.worker.finished.connect(self.handle_sn_finished)  # 处理结束
        self.worker.start()  # 启动工作线程

        self.flag_stop = False

        self.btn_start.setEnabled(False)
        self.btn_stop.setEnabled(True)
        self.btn_pause.setEnabled(True)
        self.btn_resume.setEnabled(False)
        self.text_edit.clear()

    @Slot()
    def stop(self):
        self.worker.stop()  # 调用工作线程的终止执行方法
        self.flag_stop = True
        self.text_edit.append("终止")

        self.btn_start.setEnabled(True)
        self.btn_stop.setEnabled(False)
        self.btn_pause.setEnabled(False)
        self.btn_resume.setEnabled(False)

    @Slot()
    def pause(self):
        self.worker.pause()  # 调用工作线程的暂停执行方法

        self.btn_pause.setEnabled(False)
        self.btn_resume.setEnabled(True)

    @Slot()
    def resume(self):
        self.worker.resume()  # 调用工作线程的恢复执行方法

        self.btn_pause.setEnabled(True)
        self.btn_resume.setEnabled(False)

    @Slot()
    def handle_sn_progress(self, msg):
        self.text_edit.append(msg)

    @Slot()
    def handle_sn_paused(self):
        self.text_edit.append("暂停")

    @Slot()
    def handle_sn_resumed(self):
        self.text_edit.append("继续")

    @Slot()
    def handle_sn_finished(self):
        if not self.flag_stop:
            self.text_edit.append("结束")

        self.btn_start.setEnabled(True)
        self.btn_stop.setEnabled(False)
        self.btn_pause.setEnabled(False)
        self.btn_resume.setEnabled(False)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    sys.exit(app.exec_())

```







# 事件系统

PySide2的事件处理系统是其GUI框架的核心部分，它负责接收和分发来自操作系统或用户的各种事件，并确保应用程序能够对这些事件做出响应。以下是事件处理系统的几个关键概念和组件：

1. 事件（Event）

 - 键盘事件：按键按下和松开。 
 - 鼠标事件：鼠标指针移动、鼠标按键按下和松开。 
 - 拖放事件：用鼠标进行拖放。
 - 滚轮事件：鼠标滚轮滚动。
 - 绘屏事件：重绘屏幕的某些部分。 
 - 定时事件：定时器到时。 
 - 焦点事件：键盘焦点移动。
 - 进入和离开事件：鼠标指针移入Widget内，或者移出。
 - 移动事件：Widget的位置改变。
 - 大小改变事件：Widget的大小改变。
 - 显示和隐藏事件：Widget显示和隐藏。
 - 窗口事件：窗口是否为当前窗口。

还有一些常见的事件，比如Socket事件、剪贴板事件、字体改变事件、布局改变事件等。



2. 事件过滤器（Event Filters）

事件过滤器允许一个对象监听另一个对象的事件。通过安装事件过滤器，一个对象可以在事件到达目标对象之前拦截并处理该事件。这可以用于全局监听或修改事件处理流程。



3. 事件分发（Event Dispatching）

当事件发生时，Qt的事件处理系统会根据事件类型和目标对象分发这些事件。事件首先被发送到`QCoreApplication::notify()`方法，然后转发给相应的对象。对象通过重载`event()`方法来处理这些事件。如果对象不处理该事件，则事件可能被进一步传递给其父对象或其他相关对象。



4. 事件处理方法

每个可接收事件的对象通常会有一些预定义的事件处理方法，如：

- `keyPressEvent()` 和 `keyReleaseEvent()` 处理键盘事件。
- `mousePressEvent()`、`mouseMoveEvent()` 和 `mouseReleaseEvent()` 处理鼠标事件。
- `paintEvent()` 处理重绘请求。
- `resizeEvent()` 处理窗口尺寸变化。



5. 自定义事件

除了预定义的事件类型外，开发者还可以创建自定义事件(`QEvent`的子类)，并通过`QApplication::postEvent()`或`QApplication::sendEvent()`手动分发这些事件，用于实现特定的通信机制或复杂逻辑。



示例代码

下面是一个简单的事件处理示例，展示了如何重写`QWidget`的`mousePressEvent`来响应鼠标点击事件：

```python
from PySide2.QtWidgets import QApplication, QWidget
from PySide2.QtCore import Qt
import sys


class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

    def mousePressEvent(self, event):
        if event.button() == Qt.LeftButton:
            print("Left mouse button clicked at:", event.pos())
        elif event.button() == Qt.RightButton:
            print("Right mouse button clicked at:", event.pos())
        super().mousePressEvent(event)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    widget = MyWidget()
    widget.show()
    sys.exit(app.exec_())

```

此示例中，`MyWidget`类重写了`mousePressEvent`方法来打印鼠标左键或右键点击的位置。这展示了基本的事件处理流程。









# 验证器

自带的QIntValidator 有问题，设计不好。

```python
line_edit = QLineEdit(self)
line_edit.setValidator(QIntValidator(1, 55))
```

比如上面代码，想设置只能输入1~55的整数，但是却能输入0、00000、01、99。所以只能自定义验证器。

```python
import sys

from PySide2.QtWidgets import QApplication, QMainWindow, QLineEdit
from PySide2.QtGui import QValidator


class PositiveIntegerValidator(QValidator):
    def __init__(self, minValue=0, maxValue=-1, parent=None):
        super().__init__(parent)
        self.minValue = minValue
        self.maxValue = maxValue

        if not isinstance(self.minValue, int) or not isinstance(self.maxValue, int):
            raise ValueError("参数类型有误")

        if self.minValue < 0 or self.maxValue < 0 or self.minValue >= self.maxValue:
            raise ValueError("参数值有误")

    def validate(self, input, pos):
        input = str(input)

        if input == "":
            return QValidator.Intermediate

        if input.startswith("0") and len(input) > 1:
            return QValidator.Invalid

        try:
            value = int(input)
        except ValueError:
            return QValidator.Invalid

        if value < self.minValue or (self.maxValue != -1 and value > self.maxValue):
            return QValidator.Invalid

        return QValidator.Acceptable


class MainWindow(QMainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()

        line_edit = QLineEdit(self)
        line_edit.setValidator(PositiveIntegerValidator(0, 55))


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    app.exec_()

```

以上代码就能实现只能输入设置的指定范围内的整数。



# 模型/视图编程

官方文档：https://doc.qt.io/qtforpython-5/overviews/model-view-programming.html#model-view-programming



委托示例
https://code.qt.io/cgit/pyside/pyside-setup.git/tree/examples/widgets/itemviews/stardelegate?h=5.15



# 向导页面

示例：https://code.qt.io/cgit/pyside/pyside-setup.git/tree/examples/widgets/dialogs/classwizard?h=5.15




# 打包
## PyInstaller

官方文档：https://pyinstaller.org/en/stable/index.html



```python
import shutil
import os
import time
import PyInstaller.__main__


start = time.time()

files = ["DCT.spec"]
dirs = ["build", "dist"]

for item in files:
    if os.path.exists(item):
        os.remove(item)

for item in dirs:
    if os.path.exists(item):
        shutil.rmtree(item)


command = [
    "app.py",
    "--noconfirm",
    "--onedir",
    "--windowed",
    "--clean",
    "--uac-admin",
    "--name=DCT",
    "--icon=res/logo.ico",
    "--add-data=res/dll/BaseType.dll;.",
    "--add-data=res/dll/BuffDll.dll;.",
    "--add-data=res;res",
]

PyInstaller.__main__.run(command)

end = time.time()
print(f"耗时: {int(end - start)}秒")
```
