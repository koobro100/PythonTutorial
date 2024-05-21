# 折线图



linestyles 参数用于设置线条的样式。它可以接受以下几种值：

- `solid`：实线
- `dashed`：虚线
- `dashdot`：点划线
- `dotted`：点线
- `None` 或 `空`：无线条



```python
import random

import matplotlib.pyplot as plt

# 数据    
x_data = ["1月", "2月", "3月", "4月", "5月", "6月", "7月", "8月", "9月"]
y_data = [random.randint(10, 90) for _ in range(len(x_data))]

# 标题
plt.title(title_text, fontdict=title_font_dict)

# 字典设置
# d = {
#     "title": {
#         "text": '月销售量-折线图',
#         'family': 'serif',
#         'size': 16,
#         'color': 'blue'
#     },
#     "figure": {
#         "size": (6, 4),
#         "color": "white",
#     }
# }


# def set_style(style_dict):
#     plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签  
#     plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号  
# 
#     if "title" in d:
#         if "text" in d.get("title"):
#             title_text = d.get("title").get("text")
# 
#         title_font_dict = {k: style_dict.get(k) for k in ["family", "size", "color"] if k in d.get("title")}

# plt.title(title_text, fontdict=title_font_dict)

# if "figure" in d:


# set_style(d)


# 设置图形尺寸和背景颜色  
plt.figure(figsize=(6, 4), facecolor='white')

# 创建折线图    
plt.plot(x_data, y_data, color='#3875ff', linestyle="solid", linewidth=1.8)

# 隐藏顶部和右边的边框
plt.gca().spines['top'].set_visible(False)
plt.gca().spines['right'].set_visible(False)

# 设置边框样式、宽度和颜色（统一设置）
for spine in plt.gca().spines.values():
    spine.set_linestyle('--')  # 设置边框样式为虚线
    spine.set_linewidth(1)  # 设置边框宽度
    spine.set_color('#dfdfdf')  # 设置边框颜色为绿色

# 隐藏x轴和y轴上的刻度线
plt.tick_params(axis='both', which='both', bottom=False, top=False, left=False, right=False)

# 添加标题和轴标签    
# plt.xlabel('月份')
# plt.ylabel('销售额')

# 图例  
# plt.legend(labels=["奥迪"], ncol=2)

# 设置网格线  
# plt.grid(True)

# 轴范围  
# plt.xlim(datetime.date(2022, 1, 1), datetime.date(2022, 1, 6))
plt.ylim(0, 100)

# 旋转x轴标签  
plt.xticks(color="#666")
# plt.xticks(color="#666", rotation=45)

# 设置y轴刻度间隔为10
plt.yticks(range(0, 101, 10), color="#666")

# 数据点样式  
# plt.scatter(x_data, y_data, marker='o', color='blue')

# 峰值、谷值  
max_peaks = [i for i, v in enumerate(y_data) if v == max(y_data)]
min_peaks = [i for i, v in enumerate(y_data) if v == min(y_data)]

# 数据点的数值标签  
for i in range(len(x_data)):
    # plt.scatter(x_data[i], y_data[i], marker='o', color='blue', label=f'数据点 {i + 1}')
    plt.annotate(y_data[i], (x_data[i], y_data[i]), textcoords="offset points", xytext=(0, 10),
                 ha='center', color="#0244bc")

# 标注峰值、谷值  
# for peak in max_peaks:
#     plt.annotate(y_data[peak], (x_data[peak], y_data[peak]), textcoords="offset points",
#                  xytext=(0, 20),
#                  ha='center')
# for peak in min_peaks:
#     plt.annotate(y_data[peak], (x_data[peak], y_data[peak]), textcoords="offset points",
#                  xytext=(0, 20),
#                  ha='center')

# 绘制垂直线
plt.vlines(x_data, ymin=0, ymax=y_data, linestyles="dotted", colors="#3875ff")

# 显示图形    
plt.show()
```







```python
import sys
import numpy as np
from PySide2.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QWidget
from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas
from matplotlib.figure import Figure


class MatplotlibWidget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        self.figure = Figure()
        self.canvas = FigureCanvas(self.figure)
        layout = QVBoxLayout()
        layout.addWidget(self.canvas)
        self.setLayout(layout)

    def plot(self):
        ax = self.figure.add_subplot(111)
        x = np.linspace(0, 10, 100)
        y = np.sin(x)
        ax.plot(x, y)
        ax.set_title('Matplotlib Plot')
        self.canvas.draw()


class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.central_widget = MatplotlibWidget(self)
        self.setCentralWidget(self.central_widget)
        self.central_widget.plot()


def main():
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())


if __name__ == "__main__":
    main()
```





```python
# 执行封装

import random

import matplotlib.pyplot as plt

plt.rcParams["font.sans-serif"] = ["SimHei"]  # 用来正常显示中文标签
plt.rcParams["axes.unicode_minus"] = False  # 用来正常显示负号

# 数据
x_data = ["1月", "2月", "3月", "4月", "5月", "6月", "7月", "8月", "9月"]
y_data = [random.randint(10, 90) for _ in range(len(x_data))]

# 参数
style_dict = {
    # 标题
    "title": {
        "label": "月销售量-折线图",  # 文本
        "fontdict": {
            "family": "sans-serif",  # 字体
            "size": 12,  # 大小
            "color": "b",  # 颜色
            "weight": "normal",  # 粗细
        },
        "loc": "center",  # 位置。'center', 'left', 'right'
        "pad": 20,  # 标题与图表的距离
    },
    # 图形
    "figure": {
        # "figsize": (3, 2),  # 尺寸
        # "facecolor": "red",  # 背景颜色
        # "frameon": False,  # 显示边框
        # "edgecolor": "green",  # 边框颜色
        # "dpi": 100,  # 分辨率
        # "clear": True,  # 创建新图表之前清除当前图表
    },
    # 图例
    "legend": {
        "loc": "upper left",  # 图例位置。best、upper right、upper left、lower right、lower left等
        "shadow": True,  # 图例阴影
        "borderaxespad": 1,  # 图例和图表的距离
        "frameon": True,  # 图例是否显示边框
        "framealpha": 1,  # 图例边框透明度
        "labels": ["奥迪"],  # 标签
        "labelcolor": "#333",  # 标签颜色
        "fontsize": 12,  # 标签大小
        "title": "",  # 标题
        "title_fontsize": 12,  # 标题大小
        "handlelength": 2,  # 标记长度
        "handletextpad": 0,  # 标记和文本的间距
    },
    # 折线
    "line": {
        "color": "#3875ff",  # 颜色
        "alpha": 0.5,  # 透明度。0为完全透明，1为不透明
        "linestyle": "--",  # 风格。 '-'（实线）、'--'（虚线）、'-.'（点划线）
        "linewidth": 1.8,  # 宽度
        "marker": "*",  # 标记样式。"."（点）、"o"（圆圈）、"s"（正方形）、"*"（星号）、"+"（加号）、"x"（叉号）
        "markersize": 10,  # 标记大小
        "markerfacecolor": "red",  # 标记背景颜色
        "markeredgecolor": "black",  # 标记背景颜色,
    },
    # 轴标签
    "ax_label": {
        "x_label": {
            "text": "X轴标签",
            "fontsize": 20,
        },
        "y_label": {
            "text": "Y轴标签",
            "fontsize": 20,
        },
    }
}


# 设置样式函数
def set_style(l_plt, l_x_data, l_y_data, d):
    # 参数值
    line = d.get("line")
    title = d.get("title")
    legend = d.get("legend")
    figure = d.get("figure")
    ax_label = d.get("ax_label")
    x_label = d.get("ax_label").get()

    # 轴标签
    l_plt.xlabel(ax_label.get("x_label").get("text"), **)
    l_plt.ylabel(ax_label.get("y_label").get("text"))

    # 创建、数据、折线
    l_plt.plot(l_x_data, l_y_data, **line)

    # 标题
    l_plt.title(**title)

    # 图例
    l_plt.legend(**legend)

    # 图形
    l_plt.figure(**figure)


# 设置样式
set_style(plt, x_data, y_data, style_dict)

# 显示图形
plt.show()
```





# 柱形图





```python
import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

# 学生成绩数据
grades = ["A", "B", "C", "D", "F"]
num_students = [10, 25, 40, 15, 5]  # 每个等级的学生人数

# 创建柱形图
bars = plt.bar(grades, num_students, color='skyblue', alpha=0.7)

# 添加标题和轴标签
plt.title('学生成绩分布')
plt.xlabel('等级')
plt.ylabel('人数')

# 峰值、谷值
max_y = max(num_students)
min_y = min(num_students)
max_peaks = [i for i, v in enumerate(num_students) if v == max_y]
min_peaks = [i for i, v in enumerate(num_students) if v == min_y]

# 数据点的数值标签
for i in range(len(grades)):
    plt.text(grades[i], num_students[i] + 0.5, f'{num_students[i]}', ha='center')

# 显示图形  
plt.show()
```





```python
# 三维柱形图

import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np

plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

# 学生成绩数据
grades = ["A", "B", "C", "D", "F"]
num_students = [10, 25, 40, 15, 5]  # 每个等级的学生人数

# 计算百分比
total_students = sum(num_students)
percentages = [(num / total_students) * 100 for num in num_students]

# 创建三维条形图
fig = plt.figure(figsize=(8, 6))
ax = fig.add_subplot(111, projection='3d')

# 每个等级的高度
heights = percentages

# 条形图的宽度
width = 0.5

# 每个等级的位置
positions = np.arange(len(grades))

# 创建3D条形图
bars = ax.bar(positions, heights, zs=0, zdir='y', width=width,
              color=['gold', 'lightcoral', 'lightskyblue', 'lightgreen', 'lightpink'])

# 设置x轴标签
ax.set_xticks(positions)
ax.set_xticklabels(grades)

# 设置标题
ax.set_title('学生成绩分布（3D条形图）')

# 添加数值标签
for bar, num in zip(bars, num_students):
    x, z = bar.get_x() + width / 2, bar.get_height()
    ax.text(x, z, num, ha='center', va='bottom')

# 显示图形
plt.show()
```



# 条形图



```python
import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

# 数据  
x = ['A', 'B', 'C', 'D', 'E']
y = [20, 35, 30, 27, 25]

# 创建条形图  
plt.barh(x, y)

# 添加标题和轴标签  
plt.title('条形图')
plt.xlabel('Y轴')
plt.ylabel('X轴')

# 显示图表  
plt.show()
```



# 饼图

```python
# 饼图

import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

# 学生成绩数据
grades = ["A", "B", "C", "D", "F"]
num_students = [10, 25, 40, 15, 5]  # 每个等级的学生人数

# 突出显示最大的扇形
explode = [0] * len(grades)
max_index = [i for i, v in enumerate(num_students) if v == max(num_students)][0]
explode[max_index] = 0.1

# 创建饼图
plt.figure(figsize=(5, 5))  # 设置图形大小
plt.pie(num_students, explode=explode, labels=grades, autopct='%1.1f%%',
        colors=['gold', 'lightcoral', 'lightskyblue', 'lightgreen', 'lightpink'], startangle=140, labeldistance=1.08,
        shadow=True)

# 添加标题
plt.title('学生成绩分布')

# 显示图形
plt.axis('equal')  # 使饼图比例相等，呈圆形
plt.show()
```



# 散点图

```python
import random
import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

# 创建数据  
length = 30
x = [random.randint(1, 100) for _ in range(length)]
y = [random.randint(1, 100) for _ in range(length)]

# 创建散点图  
plt.scatter(x, y)

# 添加标题和轴标签  
plt.title('散点图')
plt.xlabel('x轴')
plt.ylabel('y轴')

# 显示图形  
plt.show()
```





# 面积图

```python
import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

# 数据  
x = [1, 2, 3, 4, 5]
y = [2, 3, 4, 5, 6]

# 创建面积图  
plt.fill_between(x, y)

# 添加标题和轴标签  
plt.title('Area Chart')
plt.xlabel('X-axis')
plt.ylabel('Y-axis')

# 显示图表  
plt.show()
```





# 雷达图

```python
import matplotlib.pyplot as plt
import numpy as np

plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

# 数据  
labels = ["A", 'B', 'C', 'D', 'E']
stats = [25, 35, 30, 27, 25]

# 计算角度  
angles = np.linspace(0, 2 * np.pi, len(labels), endpoint=False)

# 为了使雷达图封闭，需要将初始数据添加到末尾   
stats = np.concatenate((stats, [stats[0]]))
angles = np.concatenate((angles, [angles[0]]))

# 创建极坐标轴  
fig, ax = plt.subplots(figsize=(6, 6), subplot_kw=dict(polar=True))

# 绘制雷达图  
ax.fill(angles, stats, color='red', alpha=0.25)

# 添加标签  
ax.set_yticklabels([])
ax.set_xticks(angles[:-1])
ax.set_xticklabels(labels)

# 添加标题  
plt.title('雷达图')

# 显示图表  
plt.show()
```

