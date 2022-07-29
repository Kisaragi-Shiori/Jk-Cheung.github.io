---
title: Matplotlib笔记
hidden: false
katex: false
date: 2022-07-27 11:49:21
id: Note-of-Matplotlib
sticky:
categories:
- 学习笔记
tags:
- 数据可视化
- Python
- 数据分析
---

<!-- more -->

## 导入与基本设置

```python
%matplotlib inline  # 设置图像内嵌显示于 Jupyter Notebook 中
%config InlineBackend.figure_format = 'svg'  # 设置图片为 svg 矢量图格式
from matplotlib import pyplot as plt  # 提供常用二维图绘制的 sublibrary
```

## 图片设置

### 图例设置

```python
plt.legend(loc=0)  # 设置图例的位置，loc=0 代表自动选择最佳位置
```

----

| loc 编号 |  loc 字符串  |
| :------: | :----------: |
|    0     |     best     |
|    1     | upper right  |
|    2     |  upper left  |
|    3     |  lower left  |
|    4     | lower right  |
|    5     |    right     |
|    6     | center left  |
|    7     | center right |
|    8     | lower center |
|    9     | upper center |
|    10    |    center    |

也可以用一个数组 (x0, y0) 来确定位置，图片的左下角为 (0, 0)，右上角为 (1, 1)。

```python
plt.legend(bbox_to_anchor=(x0, y0, width=0, height=0), loc=9)
```

对于 (x0, y0)，图片的左下角为 (0, 0)，右上角为 (1, 1)，当 `width=0， height=0` （缺省）时，将图例中和 `loc` 参数相对应的那一部分与 (x0, y0) 重合，例如 `x0=1, y0=1, loc=2` 时代表图例的左上角和图的右上角重合。

当 `width `和 `height` 不等于 0 时，以 `x0, y0, width, height` 这四个参数创建一个矩形，将图例放在该矩形中与 `loc` 参数相对应的部分。

最常用的还是只设置 x0, y0 的情况。

### 批量自定义配置

Matplotlib 可通过自定义参数来实现批量配置，相关函数为 `plt.rcParams`，其全称为「runtime configuration Parameters」，其是一个字典形式，其中常用的设置如下

```python
plt.rcParams['font.sans-serif'] = 'SimSun'  # 设置字体为宋体
plt.rcParams["axes.unicode_minus"]=False  # 设置不使用 unicode 的负号，解决负号乱码
plt.rcParams['figure.figsize'] = (5.0, 4.0)  # 设置图像最大范围
plt.rcParams['lines.linestyle'] = '-.'  # 设置线条样式
plt.rcParams['lines.linewidth'] = 3  # 设置线条宽度
```

可以将该函数理解为设置默认值，防止对图像进行多次重复的设置。

### 设置绘图风格

Matplotlib 可使用函数 `plt.style()` 来改变绘图风格，其常用命令如下

```python
plt.style.available  # 查看可用风格
plt.style.use('seaborn')  # 使用指定风格，此处为 'seaborn' 风格
```

### 主次刻度线

一个图中可以存在两个刻度线，即主刻度线和次刻度线。使用命令 `plt.minorticks_on()` 和 `plt.minorticks_off()` 两个函数即可进行开启与关闭。对于主次刻度线的设置可使用 `plt.tick_params(axis='both', **kwargs)` 函数。其中部分常用参数如下

- `axis: {'x', 'y', 'both'}`：设置的轴，默认为 `'both'`；
- `reset: bool`：默认为 `False`，确定在设置前是否恢复默认设置；
- `which: {'major', 'minor', 'both'}`：设置的尺度，默认为 `'major'`，`'major'` 表示显示主刻度线，`'minor'` 表示显示次刻度线；
- `direction: {'in', 'out', 'inout'}`：设置刻度线的位置在坐标轴内、外或者两边；
- `length, width: float`：设置刻度线的长与宽；
- `color: color`：设置刻度线的颜色；
- `bottom, top, left, right: bool`：在各方向上设置是否显示刻度线；
- `labelbottom, labeltop, labelleft, labelright: bool`：在各方向上设置是否显示刻度值；

### 网格线

如果想要在图片中显示网格，可以使用 `grid()` 函数，其命令参数为

```python
grid(b=Ture, which, axis, color, linestyle, linewidth, **kwargs)
```

其中参数作用如下

- `b`：是否显示网格线，默认为`None`。若设置 `**kwargs` 参数则值为 `True`；
- `which`：网格线显示的尺度，取值范围  `{'major', 'minor', 'both'}`，默认为 `'major'`，`'major'` 表示显示主刻度线，`'minor'` 表示显示次刻度线；
- `axis`：网格线显示的轴，取值范围为 `{'x', 'y', 'both'}`，默认为 `'both'`；
- `**kwargs`：`Line2D` 线条对象属性，即 `plt.plot()` 中的 `color` 等参数。

可通过如下代码实现实线主刻度线网格，点线次刻度线网格：

```python
plt.minorticks_on()
plt.grid(which='major', linestyle='-')
plt.grid(which='minor', linestyle=':')
```

## 二维绘图

### 折线/点图



```python
plt.plot(x, y)  # 以数组 x 和 y 绘制点图
plt.xlim([1, 10])  # 设置 x 轴的范围
plt.ylim([1, 10])  # 设置 y 轴的范围
plt.xticks(np.arange(0, 11, 1))  # 设置 x 轴显示的坐标
plt.yticks(np.arange(0, 11, 1))  # 设置 y 轴显示的坐标
plt.xlabel('x')  # 设置 x 轴的标签为 x
plt.ylabel('y')  # 设置 y 轴的标签为 y
plt.title('x-y 图')  # 设置图片标题
plt.show()  # 显示图形，在这条代码前的所有绘图都是在同一张图上的
```

对于函数 `plt.plot(x, y)`，其常用参数为 `plt.plot(x, y, [fmt], label, linestyle, linewidth, marker, markersize)`，其中 `[fmt]` 是一个字符串，由三部分组成，`fmt = '[color][marker][line]'`，比如蓝色圆点实线就表示为 `bo-`，其中 `b` 为 blue 的缩写，`o` 为圆点的象形，`-` 为实线的象形。也对这三个属性进行单独赋值：`color='blue', marker='o', linestyle='-'`。以下是 `Color` 常见参数

| Color 参数 | 代表颜色  |
| :--------: | :-------: |
|    'b'     |  'blue'   |
|    'r'     |   'red'   |
|    'g'     |  'green'  |
|    'c'     |  'cyan'   |
|    'm'     | 'megenta' |
|    'y'     | 'yellow'  |
|    'k'     |  'black'  |
|    'w'     |  'white'  |

以下是 `Marker` 常见参数

| marker 参数 |   代表 Marker    |
| :---------: | :--------------: |
|     '.'     |     'point'      |
|     ','     |     'pixel'      |
|     'o'     |     'circle'     |
|     'v'     | 'triangle_down'  |
|     '^'     |  'triangle_up'   |
|     '<'     | 'triangle_left'  |
|     '>'     | 'triangle_right' |
|     '1'     |    'tri_down'    |
|     '2'     |     'tri_up'     |
|     '3'     |    'tri_left'    |
|     '4'     |   'tri_right'    |
|     's'     |     'square'     |
|     'p'     |    'pentagon'    |
|     '*'     |      'star'      |
|     'h'     |    'hexagon1'    |
|     'H'     |    'hexagon2'    |
|     '+'     |      'plus'      |
|     'x'     |       'x'        |
|     'D'     |    'diamond'     |
|     'd'     |  'thin_diamond'  |
|    '\|'     |     'vline'      |
|     '_'     |     'hline'      |

以下是 Line 常见参数

| Line 参数 | 代表Line  |
| :-------: | :-------: |
|    '-'    |  'solid'  |
|   '--'    | 'dashed'  |
|   '-.'    | 'dashdot' |
|    ':'    | 'dotted'  |

在一个 plot 中，可以绘制多个点图，通过设置 `label` 标签以及 `plt.legend()` 命令来进行区分。

```python
plt.minorticks_on()
plt.grid(which='major', linestyle='-')
plt.grid(which='minor', linestyle=':')
```

### 散点图

Matpltlib 使用 `plt.scatter()` 函数来实现散点图的绘制，其具体参数如下

```python
plt.scatter(x, y, s=None, c=None, marker=None, cmap=None, norm=None, vmin=None, vmax=None, alpha=None, linewidths=None, *, edgecolors=None, plotnonfinite=False, data=None, **kwargs)
```

- `x, y: ArrayLike`：绘图坐标数据；
- `s: float, ArrayLike`：点的大小，默认 `20`，为 `float` 时为所有点设置相同大小，为 `ArrayLike` 类型时为每个点对应设置大小；
- `c: str, ArrayLike`：点的颜色，默认为 `'b'`，同 `s` 可为 `ArrayLike` 类型。
- `cmap, norm`：一般忽略，高级颜色设置；
- `vmin, vmax`：一般忽略，亮度设置；
- `alpha: float`：透明度设置，0-1 之间，默认为 None，即不透明；
- `linewidths: float, ArrayLike`：标记点的长度（半径）设置；
- `edgecolor: {'face', 'none', None}`：边缘颜色设置，一般忽略；
- `plotnonfinite: bool`：设置是否使用非有限的 `c` (inf, -inf, nan) 进行图像的绘制。

## 统计分析图

### 柱状图

#### 竖直柱状图

Matplotlib 使用如下函数实现竖直柱状图的绘制，其常用参数如下

```python
matplotlib.pyplot.bar(x, height, width=0.8, bottom=None, *, align='center', data=None, **kwargs)
```

- `x, hight: ArrayLike`：x 轴的位置序列与 y 轴的数值（高度）序列；
- `width: float, ArrayLike`：柱形的宽度，为 `float` 类型时为所有柱形设置，为 `ArrayLike` 时对应设置；
- `bottom: float, ArrayLike`：柱形的底部所在高度，为 `float` 类型时为所有柱形设置，为 `ArrayLike` 时对应设置；
- `aligh: {'center', 'edge'}`：柱形的对齐方式，默认为 `'center'`，即将柱形中心与 `x` 坐标对齐，若为 `'edge'` 则将偏左侧与 `x` 坐标对齐，如果想做到偏右侧则设置 `width` 为负值即可；
- `alpha: float`：同散点图；
- `color, edgecolor`：同散点图；
- `label: ArrayLike`：柱形的标签，同 `plt.plot()`；
- `lw: float`：边缘或线的宽度。

如果想要绘制重合对比图，可以在同一图上同时绘制两柱状图，但是要注意遮挡问题，因此可将上层图像设置为部分透明，例如

```python
x = np.linspace(0, 2 * np.pi, 10)
y = np.sin(x)
z = np.cos(x)
plt.bar(x, z, width=0.7)
plt.bar(x, y, width=0.7, alpha=0.7)  # 上层图形
plt.show()
```

但是如果想要两柱形并列显示，就需要进行如下设置

```python
x = np.linspace(0, 2 * np.pi, 10)
y = np.sin(x)
z = np.cos(x)
bar_width = 0.5
plt.bar(np.arange(len(x)), z, width=bar_width)
plt.bar(np.arange(len(x)) + bar_width, y, width=bar_width, alpha=0.7)
plt.show()
```

将两个图的横坐标错开一个柱形的宽度。或者使用

```python
x = np.linspace(0, 2 * np.pi, 10)
y = np.sin(x)
z = np.cos(x)
bar_width = 0.3
plt.bar(x, z, width=bar_width, align='edge')
plt.bar(x, y, width=-bar_width, align='edge', alpha=0.7)
plt.show()
```

利用 `aligh` 参数来进行设置

#### 水平柱状图

水平柱状图使用以下函数调用

```python
matplotlib.pyplot.barh(y, width, height=0.8, left=None, *, align='center', **kwargs)
```

其余与竖直类似。

### 直方图

Matplotlib 中使用如下函数实现绘制直方图

```python
plt.hist(x, bins=None, range=None, density=False, weights=None, cumulative=False, bottom=None, histtype='bar', align='mid', orientation='vertical', rwidth=None, log=False, color=None, label=None, stacked=False, *, data=None, **kwargs)
```

- `x: ArrayLike`：需要绘制的原数据，可以是一个 `ArrayLike` 组成的 `ArrayLike`，形成不同的数据集，不同的数据集不需要长度相同；
- `bins: int, str, ArrayLike`：如果为 `int` 类型，则指定箱子（即柱形）个数，默认为 10；`str` 类型不常见，请查看文档；如果为长度为 `l` 的 `ArrayLike` 类型，则将其中的数值作为分隔点形成 `l-2 个左闭右开的区间以及一个（最后一个）闭区间；
- `range: ArrayLike`：含有两个数值，分别对应上下界，若 `x` 中提供的数值在上下界之外则认为为异常值不进行考虑。在 `bins` 是一个 `ArrayLike` 时不起作用；
- `density: bool`：是否显示占比，默认为 `False`；
- `weight: ArrayLike`：`x` 的权重，如果 `density` 为 `True`，则此参数为归一化；
- `cumulative: bool, -1`：是否累加，如果与 `density` 同为 `True` 则累加结果会归一化；如果该参数为 `-1` 则累加会取负；
- `bottom: float, ArrayLike`：同柱状图；
- `histtype: {'bar', 'barstacked', 'step', 'stepfilled'}`：直方图的样式，`'bar'`  为最基本的直方图，如果 `x` 输入了多个数据集，则将并列显示；`barstacked` 在多个数据集时会将它们进行堆叠；`step` 以线图进行显示；`stepfilled` 则将线图进行填充；
- `align: {'left', 'mid', 'right'}`：类似柱状图；
- `orientation: {'vertical', 'horizontal'}`：设置显示方向，类似 `plt.bar()` 与 `plt.barh()`；
- `rwidth: float, None`：柱形的宽度参数，以 `width` 为标准，默认为 `None`，若 `histtype` 为 `step` 或 `stepfilled` 则忽略此参数；
- `log: bool`：设置对数刻度，默认为 `False`；
- `color: color, ArrayLike`：同柱状图；
- `label: str, None`：直方图的标签；
- `stacked: bool`：是否堆叠。

### 饼图

Matplotlib 中使用如下函数实现绘制饼图

```python
plt.pie(x, explode=None, labels=None, colors=None, autopct=None, pctdistance=0.6, shadow=False, labeldistance=1.1, startangle=0, radius=1, counterclock=True, wedgeprops=None, textprops=None, center=(0, 0), frame=False, rotatelabels=False, *, normalize=True, data=None)
```

- `x: ArrayLike`：同直方图；
- `explode: ArrayLike`：离心距离；
- `labels: ArrayLike`：每一部分的标签；
- `color: ArrayLike`：每一部分的颜色，默认为 `None` 时使用现在设置的颜色环；
- `autopct: str, callable`：自动显示每部分所占百分比，默认为 `None` 时不显示，为 `str` 格式且非格式化字符串时则显示其中的文字，若为格式化字符串则显示对应格式的百分比；为 `callable` 时输出对应百分比的函数值；
- `pctdistance: float`：`autopct` 文字离心距离，以一个半径为参考，默认为 `0.6`；
- `shadow: bool`：是否显示阴影；
- `normalize: bool`：是否将 `x` 归一化，默认为 `True`，若为 `False` 时 `sum(x) < 1` 则绘制部分饼图，`sum(x) > 1` 则会抛出异常；
- `labeldistance: float`：`label` 文字的离心距离，类似 `pctdistance`；
- `startangle: float`：开始绘制角度，默认为 `0`，即 x 轴正半轴；
- `radius:float` ：饼图的半径；
- `counterclock: bool`：绘制方向，默认为 `True` 时为逆时针；
- `wedgeprops: dict`：饼图楔形具体参数，如 `wedgeprops={'linewidth': 3}` 设置楔形线宽为 `3`；
- `textprops: dict`：饼图文字具体参数，类似 `wedgeprops`；
- `center: ArrayLike`：饼图的圆心坐标；
- `frame: bool`：是否绘制坐标轴；
- `rotatelabels: bool`：是否旋转标签，默认为 `False`；
- `data: indexable`：不清楚，但是没什么用。

### 箱线图

Matplotlib 中使用如下函数实现绘制箱线图

```python
plt.boxplot(x, notch=None, sym=None, vert=None, whis=None, positions=None, widths=None, patch_artist=None, bootstrap=None, usermedians=None, conf_intervals=None, meanline=None, showmeans=None, showcaps=None, showbox=None, showfliers=None, boxprops=None, labels=None, flierprops=None, medianprops=None, meanprops=None, capprops=None, whiskerprops=None, manage_ticks=True, autorange=False, zorder=None, *, data=None)
```

- `x: ArrayLike`：数据；
- `notch: bool`：是否以凹口形式展现箱线图；
- `sym: str`：异常点的参数，类似 `plt.plot()` 中的 `[fmt]`；
- `vert: bool`：是否需要将箱线图垂直摆放，默认为 `True`；
- `whis: float, ArrayLike, str`：指定上下须与上下四分位的距离，如果为 `float` 类型，则设置离群值为范围外 `whis * IQR` 范围的数值，默认为 `1.5`，一般地可设置为 `3` 以寻找极端离群值；若为 `ArrayLike` 类型，则设置离群值为固定分位点，如 `[5, 95]` 设置离群值为 5 百分分位数与 95 百分分位数之外；若为 `str` 类型，则可设置其为 `range` 来强制不出现离群点；
- `positions: ArrayLike`：设置箱线图的位置；
- `widths: ArrayLike`：设置箱线图的宽度；
- `patch_artist: bool`：设置箱体是否显示颜色，默认为 `False`；
- `meanline: bool`：是否显示均值线，默认为 `False`；
- `showmeans: bool`：是否显示均值，默认为 `False`；
- `showcaps: bool`：是否显示箱线图顶端和末端的两条线，默认为 `True`；
- `showbox: bool` ：是否显示箱线图的箱体，默认为 `True`；
- `showfliers: bool`：是否显示异常值，默认为 `True`；
- `boxprops: dict`：设置箱线图的属性，如 `boxprops = {'color':'orangered','facecolor':'pink'}` 设置线颜色为橙色，填充颜色为粉色；
- `labels: ArrayLike`：为箱线图添加标签；
- `filerprops: dict`：设置异常值的属性；
- `medianprops: dict`：设置中位数的属性；
- `meanprops: dict`：设置均值的属性；
- `capprops: dict`：设置箱线图顶端和末端线条的属性；
- `whiskerprops: dict`：设置须的属性。

### 茎叶图

Matplotlib 中使用如下函数实现绘制茎叶图

```python
stem(*args, linefmt=None, markerfmt=None, basefmt=None, bottom=0, label=None, use_line_collection=True, orientation='vertical', data=None)
```

- `locs: ArrayLike`：类似箱线图的 `positions` 参数；
- `heads: ArrayLike`：对于垂直茎叶图，该参数即茎头的 y 值，对于水平茎叶图，该参数即茎头的 x 值；
- `linefmt: str`：线的参数，类似 `plt.plot()` 中的 `[fmt]`；
- `markerfmt: str`：点的参数，类似 `plt.plot()` 中的 `[fmt]`；
- `basefmt: str`：基线的参数，类似 `plt.plot()` 中的 `[fmt]`；
- `orientation: {'vertical', 'horizontal'}`：茎叶图的方向，默认为 `'vertical'`，
- `bottom: float`：基线的位置，默认为 `0`；
- `label: str`：图例中使用的标签；
- `use_line_collection: bool`：未来可能被废弃；
- `data: indexable`：不清楚，但是没什么用。

### 小提琴图

Matplotlib 中使用如下函数实现绘制茎叶图小提琴图

```python
plt.violinplot(dataset, positions=None, vert=True, widths=0.5, showmeans=False, showextrema=True, showmedians=False, points=100, bw_method=None, *, data=None)
```

- `dataset: ArrayLike`：数据；
- `positions, vert, widths, showmeans`：类似箱线图；
- `showextrema: bool`：是否显示最值，默认为 `Ture`；
- `showmedians: bool`：是否显示中位数，默认为 `False`；
- `points: int`：估计高斯核密度的点数，默认为 `100`；
- `bw_method: str, scalar, callable`：估算带带宽的方法。

