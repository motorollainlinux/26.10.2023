```Matplotlib``` - это основная библиотека для построения научных графиков в Python.
Включает функции для создания высококачественных
визуализаций: линейных диаграмм, гистограмм и т.д. Визуализация данных и результатов - цель использования библиотеки matplotlib. 
При работе в среде можно вывести рисунок на экран с помощью встроенных команд:
+ ```%matplotlib notebook``` для визуализации графика в интерактивном режиме;
+ ```%matplotlib inline``` для получения статичного изображения.<br>

Чтобы нарисовать гистограмму, нужно вызвать всего одну команду: ```matplotlib.pyplot.hist(arr)``` Гистрограмма состоит из
повоторяющихся по форме фигур - прямоугольников. Чтобы нарисовать прямоугольник, нужно знать координату одного угла и ширину/
длину. Прямоугольник мы бы рисовали линиями, соединяя угловые точки. Этот пример отображает иерархичность рисунков, когда итоговая
диаграмма (высокий уровень) состоит из простых геометрических фигур (более низкий, средний уровень), созданных несколькими
универсальными методами рисования (низкий уровень).

## Иерархическая структура рисунка
Пользовательская работа подразумевает операции с разными уровнями: ```Figure(Рисунок) -> Axes(Область рисования) ->
Axis(Координатная ось)```
1. Рисунок (Figure)

Любой рисунок в matplotlib имеет вложенную структуру. Рисунок - это объект самого верхнего уровня, на котором располагаются:
+ области рисования (Axes);
+ элементы рисунка Artists (заголовки, легенда и т.д.);
+ основа-холст (Canvas).

На рисунке может быть несколько областей рисования Axes, но данная область рисования Axes может принадлежать только одному
рисунку Figure.

2. Область рисования (Axes)

Объект среднего уровня. Это часть изображения с пространством данных. Каждая область рисования Axes содержит две (или три в случае
трёхмерных данных) координатных оси (Axis объектов), которые упорядочивают отображение данных.

3. Координатная ось (Axis)

Координатная ось является объектом среднего уровня, которая определяет область изменения данных. На них наносятся:
+ деления ticks;
+ подписи к делениям ticklabels.

Расположение делений определяется объектом Locator, а подписи делений обрабатывает объект Formatter. Конфигурация координатных
осей заключается в комбинировании различных свойств объектов Locator и Formatter.

4. Элементы рисунка (Artists)

Практически всё, что отображается на рисунке является элементом рисунка (Artist), даже объекты Figure, Axes и Axis. Элементы рисунка
Artists включают в себя такие простые объекты как:
+ текст (Text);
+ плоская линия (Line2D);
+ фигура (Patch) и другие.

Когда происходит отображение рисунка (figure rendering), все элементы рисунка Artists наносятся на основу-холст (Canvas). Большая часть
из них связывается с областью рисования Axes. Также элемент рисунка не может совместно использоваться несколькими областями Axes
или быть перемещён с одной на другую.

## Pyplot

Pyplot - интерфейс для построения графиков простых функций. Позволяет пользователю сосредоточиться на выборе готовых решений и
настройке базовых параметров рисунка. Это его главное достоинство, поэтому изучение matplotlib лучше всего начинать именно с
интерфейса pyplot.
Существует стандарт вызова pyplot в python:

```
import matplotlib.pyplot as plt
```

Рисунки в matplotlib создаются путём последовательного вызова команд. Графические элементы (точки, линии, фигуры и т.д.)
наслаиваются одна на другую последовательно. При этом последующие перекрывают предыдущие, если они занимают общее участки на
рисунке (регулируется параметром zorder).

В matplotlib работает правило "текущей области" ("current axes"), которое означает, что все графические элементы наносятся на текущую
область рисования. Несмотря на то, что областей рисования может быть несколько, один из них всегда является текущей. Так как главным
объектом в matplotlib является рисунок Figure, создание научной графики нужно начинать именно с создания рисунка. Создать рисунок
figure позволяет метод ```plt.figure()```.

После вызова любой графической команды (функции), которая создаёт какой-либо графический объект, например, ```plt.scatter()``` или ```plt.plot()```,
всегда существует хотя бы одна область для рисования (по умолчанию прямоугольной формы). Чтобы текущее состояние рисунка
отразилось на экране, можно воспользоваться командой ```plt.show()```. Будут показаны все рисунки (figures), которые были созданы.

```diff
import matplotlib.pyplot as plt
# Создание объекта Figure
fig = plt.figure() 
# тип объекта Figure
print (type(fig))
# scatter - метод для нанесения маркера в точке (1.0, 1.0)
plt.scatter(1.0, 1.0) 
print (fig.axes)
# После нанесения графического элемента в виде маркера, список текущих областей состоит из одной области
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/dc1d6d8c-06ac-44de-b7bb-d297f5768866)

Обычно рисунок в matplotlib представляет собой прямоугольную область, заданную в относительных координатах: от 0 до 1 включительно
по обеим осям. Второй распространённый вариант типа рисунка - круглая область (polar plot).

Чтобы сохранить получившийся рисунок можно воспользоваться методом ```plt.savefig()``` - сохранение текущей конфигурации текущего
рисунка в графический файл с некоторым расширением (png, jpeg, pdf и др.), который можно задать через параметр fmt. Например,
```plt.savefig('{}.{}'.format(name, fmt), fmt='png')```.

Её нужно вызывать в конце исходного кода, после вызова всех других команд. Если в python-скрипте создать несколько рисунков figure и
попытаться сохранить их одной командой ```plt.savefig()```, то будет сохранён последний рисунок figure.

## Текст

Текст является одним из базовых графических элементов рисунка в matplotlib. Подписи координатных осей и их делений, заголовки,
пояснительные подписи на графиках и диаграммах - это всё текст. В Matplotlib возможна поддержка кириллицы для создания научной
графики с подписями на русском языке. Одним из весомых преимуществ matplotlib при работе с текстом является простая поддержка
математических формул с помощью LaTex.

## Работа с текстом

Matplotlib имеет обширную текстовую поддержку, включая поддержку математических выражений, поддержку truetype для растровых и
векторных выходных данных, разделенный новой строкой текст с произвольными поворотами и поддержкой юникода. Matplotlib также
осуществляет поддержку FreeType, что обеспечивает создание красивых, сглаженных шрифтов. matplotlib включает в себя собственный
```matplotlib.font_manager```, который реализует кросс-платформенный, W3C-совместимый алгоритм поиска шрифтов.

Matplotlib встраивает шрифты непосредственно в выходные документы, поэтому то, что вы видите на экране, - это то, что вы получаете в
печатном виде. Matplotlib предоставляет пользователю большой контроль над свойствами текста (размер шрифта, вес шрифта,
расположение и цвет текста и т. д.)

## Файл настройки matplotlibrc

matplotlibrc - файл настройки в котором хранятся значения по умолчанию для разных свойств графических элементов. Он
инициализируется при каждой загрузке модуля matplotlib. Изменив содержание файла matplotlibrc можно сохранить пользовательские
настройки для работы при следующих загрузках модуля matplotlib.

matplotlibrc представляет собой текстовый файл, каждая строка которого описывает параметры в виде:

+ параметр : значение

При этом некоторые параметры имеют вложенную структуру:

+ параметр.подпараметр1 : значение параметр.подпараметр2 : значение

Файл matplotlibrc имеет стандартный вид, ниже представлен пример организации файла:

```diff
 #...
#### CONFIGURATION BEGINS HERE
# The default backend; one of GTK GTKAgg GTKCairo GTK3Agg GTK3Cairo
# MacOSX Qt4Agg Qt5Agg TkAgg WX WXAgg Agg Cairo GDK PS PDF SVG
# Template.
# You can also deploy your own backend outside of matplotlib by
# referring to the module name (which must be in the PYTHONPATH) as
# 'module://my_backend'.
#backend : TkAgg
# If you are using the Qt4Agg backend, you can choose here
# to use the PyQt4 bindings or the newer PySide bindings to
# the underlying Qt4 toolkit.
#backend.qt4 : PyQt4 # PyQt4 | PySide
# Note that this can be overridden by the environment variable
# QT_API used by Enthought Tool Suite (ETS); valid values are
# "pyqt" and "pyside". The "pyqt" setting has the side effect of
# forcing the use of Version 2 API for QString and QVariant.
# The port to use for the web server in the WebAgg backend.
# webagg.port : 8888
# If webagg.port is unavailable, a number of other random ports will
# be tried until one that is available is found.
# webagg.port_retries : 50
# When True, open the webbrowser to the plot that is shown
# webagg.open_in_browser : True
# When True, the figures rendered in the nbagg backend are created with
# a transparent background.
# nbagg.transparent : False
# if you are running pyplot inside a GUI and your backend choice
# conflicts, we will automatically try to find a compatible one for
# you if backend_fallback is True
#backend_fallback: True
#interactive : False
#toolbar : toolbar2 # None | toolbar2 ("classic" is deprecated)
#timezone : UTC # a pytz timezone string, e.g., US/Central or Europe/Paris
# Where your matplotlib data lives if you installed to a non-default
# location. This is where the matplotlib fonts, bitmaps, etc reside
#datapath : /home/jdhunter/mpldata
### LINES
# See http://matplotlib.org/api/artist_api.html#module-matplotlib.lines for more
# information on line properties.
#lines.linewidth : 1.5 # line width in points
#lines.linestyle : - # solid line
#lines.color : C0 # has no affect on plot(); see axes.prop_cycle
#lines.marker : None # the default marker
#lines.markeredgewidth : 1.0 # the line width around the marker symbol
#lines.markersize : 6 # markersize, in points
#lines.dash_joinstyle : miter # miter|round|bevel
#lines.dash_capstyle : butt # butt|round|projecting
#lines.solid_joinstyle : miter # miter|round|bevel
#lines.solid_capstyle : projecting # butt|round|projecting
#lines.antialiased : True # render lines in antialiased (no jaggies)
# The three standard dash patterns. These are scaled by the linewidth.
#lines.dashed_pattern : 2.8, 1.2
#lines.dashdot_pattern : 4.8, 1.2, 0.8, 1.2
#lines.dotted_pattern : 1.1, 1.1
#lines.scale_dashes : True
#markers.fillstyle: full # full|left|right|bottom|top|none
### PATCHES
# Patches are graphical objects that fill 2D space, like polygons or
# circles. See
# http://matplotlib.org/api/artist_api.html#module-matplotlib.patches
# information on patch properties
#patch.linewidth : 1 # edge width in points.
#patch.facecolor : C0
#patch.edgecolor : black # if forced, or patch is not filled
#patch.force_edgecolor : False # True to always use edgecolor
#patch.antialiased : True # render patches in antialiased (no jaggies)
### HATCHES
#hatch.color : k
#hatch.linewidth : 1.0
### Boxplot
#boxplot.notch : False
#boxplot.vertical : True
#boxplot.whiskers : 1.5
#boxplot.bootstrap : None
#boxplot.patchartist : False
#boxplot.showmeans : False
#boxplot.showcaps : True
#boxplot.showbox : True
#boxplot.showfliers : True
#boxplot.meanline : False
#...
```

Чтобы отобразить откуда был загружен используемый файл matplotlibrc,используйте ```matplotlib_fname()```:
```
import matplotlib as plt
plt.matplotlib_fname()
```

Чтобы просмотреть текущие настройки введите следующее:
```
plt.rcParams
```

## Шрифты. Стили и форматы

В настройках matplotlibrc или rcParams существует такой параметр как fonts, то есть шрифты. Всего существует 5 наборов шрифтов:
+ cursive;
+ fantasy;
+ monospace;
+ sans-serif;
+ serif.

Один из этих пяти наборов является текущим. За это отвечает параметр font.family. Каждый набор может состоять из одного или более
шрифтов. Данная настройка определяет шрифт для всех подписей и текста на рисунке. Если конкретную подпись необходимо сделать
другим шрифтом, можно указать шрифт из текущего стиля прямо в команде, передав в качестве соответствующего параметра словарь:

```{'fontname':'название_шрифта'}```

Помимо семейств, текст также может иметь стиль. Атрибут стиля style может быть либо 'italic', либо 'oblique', либо 'normal' (по умолчанию).
Толщина или "жирность" шрифта, может быть задана через атрибут fontweight, который принимает значения 'bold', 'light' или 'normal' (по
умолчанию). Стили и форматы можно комбинировать.
```diff
import matplotlib as mpl
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline
mpl.rcParams['font.fantasy'] = 'Arial', 'Times New Roman', 'Tahoma', 'Comic Sans MS', 'Courier'
mpl.rcParams['font.family'] = 'fantasy'
# Текущий стиль-семейство шрифтов
cfam = mpl.rcParams.get('font.family')[0]
print('cfam %s' % cfam)
cfont = mpl.rcParams.get('font.fantasy')[0]
# Первый шрифт в текущем семействе
print(mpl.rcParams.get('font.%s' % cfam))
N = 100
x = np.arange(N)
# Задаём выборку из Гамма-распредления с параметрами формы=1. и масштаба=3.0
y = np.random.gamma(1.0, 3.0, N)
fig = plt.figure()
cc = plt.hist(y) 
text_style = ['italic', 'oblique', 'normal']
font_weights = ['bold', 'light', 'normal']
for i, ts in enumerate(text_style):
 plt.text(6, 20-5*i, '%s text style' % ts, {'fontname':'Courier'}, style=ts, fontsize=14)
 plt.text(6, 35-4*i, '%s & %s text style' % (ts, font_weights[i]), {'fontname':'Courier'}, 
 style=ts, fontweight=font_weights[i], fontsize=12)
 
plt.title('Title has %s font' % cfont, fontweight='normal', color='k', fontsize=16)
plt.xlabel('Bold weight', {'fontname':'Times New Roman'}, fontweight='bold', fontsize=16)
plt.ylabel('Light weight', {'fontname':'Times New Roman'}, fontweight='light', fontsize=14)
plt.grid(True)
plt.show()
```

![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/2164a9e7-6804-4b03-aec8-45579ba05a78)

## Поддержка LaTex
### Для работы с LaTex необходимо установить его дистрибутив

В matplotlib осуществленна безбарьерная работа с LaTex. Нужно, чтобы в системе был установлена библиотека LaTeX. Код LaTex
оформляется в виде "raw" строки (перед строкой ставится символ r -> r'строка'). Также в настройках pyplot нужно указать, чтобы текст
отрисовывался с учётом синтаксиса LaTeX. Это можно сделать через ```plt.rc('text', usetex=True)```.

## Текст на рисунке

Одними из самых базовых графических команд являются команды, отображающие текст. Такой командой, не привязаной к какому-либо
объекту вроде координатной оси или делений координатной оси, является команда ```plt.text()```.

В качестве входных данных она принимает координаты положения будущей строки и сам текст в виде строки. По умолчанию координаты
положения строки будут приурочены к области изменения данных. Можно задать положение текста в относительных координатах, когда
вся область рисования изменяется по обеим координатным осям от 0 до 1 включительно.

Таким образом, координата (0.5, 0.5) в относительных координатах означает центр области рисования. Текст можно выровнять с помощью
параметров ```horizontalalignment``` и ```verticalalignment```, а также заключать его в рамку с цветным фоном, передав параметр bbox. Bbox - это
словарь, работающий со свойствами прямоугольника (объектом Rectangle).

Помимо метода text() существуют и другие методы отображения текста в pyplot. Ниже представлен список текстовых команд в pyplot.
+ plt.xlabel() - добавляет подпись оси абсции OX;
+ plt.ylabel() - добавляет подпись оси ординат OY;
+ plt.title() - добавляет заголовок для области рисования Axes;
+ plt.figtext() - добавляет текст на рисунок Figure;
+ plt.suptitle() - добавляет заголовок для рисунка Figure;
+ plt.annotate() - добавляет примечание, которое состоит из текста и необязательной стрелки в указанную область на рисунке.

Каждый текст на рисунке - это экземпляр либо исходного класса, либо дочернего класса для ```matplotlib.text.Text```.

### Рассмотрим представленные комманды на простейшем графике y=x^2:

Зададим точки. Абсциссы определяются в диапозоне (-10.,10.), таким образом, чтобы всего получилось 20 точек. Ординаты расчитываются
через уравнение y=x^2
```
import matplotlib.pyplot as plt
import numpy as np
x = np.linspace(-10, 10, 20)
y = x**2;
```

Изобразим на графике.Ниже представлен простейший график, без редактирования и работы с текстом.
```
plt.plot(x, y, color='red', label='Линия 1');
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/c7a24917-f0d9-4a61-bfbb-7390bbefd740)

Нанесем на график сетку, добавим подписи к осям и заголовок.
```
plt.plot(x, y, color='red', label='Линия 1');
plt.grid() # сетка
plt.xlabel('ось x', fontsize=14) # добавляем подпись к оси абцисс "ось х"
plt.ylabel('ось y', fontsize=14) # добавляем подпись к оси ординат "ось y"
plt.title(r'График функций $y = x ^2$', fontsize=16, y=1.05); # доббавляем заголовок к графику "График функции y=x^2
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/9879f5e5-fe2b-465d-8aea-ea4feeade55b)

Также matplotlib позволяет нанести на график подписи с помощью функций text() и annotate().

Функция text() позволяет нанести подпись в любом месте изображение, для этого указываются координаты, где должен располагаться
текст.

Добавим подпись к точке экстремума с помощью функции text():
```
plt.plot(x, y, color='red', label='Линия 1');
plt.grid() # сетка
plt.xlabel('ось x', fontsize=14) # добавляем подпись к оси абцисс "ось х"
plt.ylabel('ось y', fontsize=14) # добавляем подпись к оси ординат "ось y"
plt.title(r'График функций $y = x ^2$', fontsize=16, y=1.05); # доббавляем заголовок к графику "График функции y=x^2
plt.text(-2., 10., 'Минимум',fontsize=12); # добавляем подпись к графику в точке (-2.5, 10)
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/11d3e6a8-b2ec-4278-a3af-963952ce8626)

Функция annotate() позволяет нанести подпись с необезательной стрелкой. Параметр xy определяет координаты точки, которую
необходимо подписать(конечная точка стрелки), параметр xytext определяет координаты точки, в которой будет располагаться текст.
```
plt.plot(x, y, color='red', label='Линия 1');
plt.grid() # сетка
plt.xlabel('ось x', fontsize=14) # добавляем подпись к оси абцисс "ось х"
plt.ylabel('ось y', fontsize=14) # добавляем подпись к оси ординат "ось y"
plt.title(r'График функций $y = x ^2$', fontsize=16, y=1.05); # доббавляем заголовок к графику "График функции y=x^2
plt.annotate("Минимум", xy=(0.,0.), xytext=(-1.9,70.),fontsize=12, arrowprops = dict(arrowstyle = '->',color = 'red')
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/054c49c4-301d-493d-bc51-f6d74f047592)

## Расположение текста на графике
```
import matplotlib as mpl
import matplotlib.pyplot as plt
import numpy as np
a = 1.
x = np.arange(0., 2*np.pi, 0.1)
```

Текст в координатах данных
```diff
xz = a*(np.sin(x) - np.cos(2*x))
fig = plt.figure()
plt.plot(xz)
# выравнивание текста по левому краю
str1 = plt.text(-np.pi/2., np.pi/2., '(sin(x)-cos(2*x))', fontsize=14) 
print('Text class: %s' % str1.__class__)
plt.grid()
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/1ec34cb0-77b3-4ebc-82b6-bebe22f66a6d)

Текст в рамке 
```diff
xz = a*(np.sin(x) - np.cos(2*x))
fig = plt.figure()
plt.plot(xz)
plt.text(2., 2., 'sin(x)-cos(2*x))', fontsize=14,
# выравнивание по вертикали и по горизонтали
horizontalalignment='center', verticalalignment='center',
bbox=dict(facecolor='pink', alpha=1.0))
plt.grid()
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/600ba05f-8c43-4dd1-9569-2b5feda94738)

Текст в относительных координатах области рисования ax
```diff
xz = a*(np.sin(x) - np.cos(2*x))
fig = plt.figure()
plt.plot(xz)
# область рисования ax
ax = fig.add_subplot(111) 
plt.text(0.5, 0.5, 'sin(x) - cos(2*x)', fontsize=14,
horizontalalignment='center', verticalalignment='center',
transform=ax.transAxes)
plt.grid()
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/3173d6f9-e389-48c8-b409-c05dab85c2e7)

## Построение графиков с помощью matplotlib
### Подключение библиотеки
```
import matplotlib.pyplot as plt
# %matplotlib inline
```
### Основные графические команды
Графические команды - это функции, которые, принимая некоторые параметры, возвращают какой-то графический результат. Это может
быть текст, линия, график, диаграмма и др. Рассмотрим графические команды, которые создают графику высокого уровня: графики или
диаграммы.

В Matplotlib заложены как простые графические команды, так и достаточно сложные. Доступ к ним через pyplot означает использование
синтаксиса вида "plt.название_команды()".

Наиболее распространённые команды для создания научной графики в matplotlib:

  1. Самые простые графические команды
     + plt.scatter() - маркер или точечное рисование;
     + plt.plot() - ломаная линия;
     + plt.text() - нанесение текста.
  2. Диаграммы
     + plt.bar(), plt.barh(), plt.barbs(), broken_barh() - столбчатая диаграмма;
     + plt.hist(), plt.hist2d(), plt.hlines - гистограмма;
     + plt.pie() - круговая диаграмма;
     + plt.boxplot() - "ящик с усами" (boxwhisker);
     + plt.errorbar() - оценка погрешности, "усы".
  3. Изображения в изолиниях
     + plt.contour() - изолинии;
     + plt.contourf() - изолинии с послойной окраской.
  4. Отображения
     + plt.pcolor(), plt.pcolormesh() - псевдоцветное изображение матрицы (2D массива);
     + plt.imshow() - вставка графики (пиксели + сглаживание);
     + plt.matshow() - отображение данных в виде квадратов.
  5. Заливка
     + plt.fill() - заливка многоугольника;
     + plt.fill_between(), plt.fill_betweenx() - заливка между двумя линиями.
  6. Векторные диаграммы
     + plt.streamplot() - линии тока;
     + plt.quiver() - векторное поле.
       
В списке нет команд для рисования различных геометрических фигур (круги, многоугольники и т.д.). Это связано с тем, что в matplotlib они
вызываются через matplotlib.patches.

Ниже показаны примеры графики, которую matplotlib создаёт "по умолчанию" при вызове той или иной графической команды.

### 1. Простые графические команды
```
import matplotlib.pyplot as plt
fig = plt.figure()
```
Добавление на рисунок прямоугольной (по умолчанию) области рисования
```
scatter1 = plt.scatter(0.0, 1.0)
print('Scatter: ', type(scatter1))
grid1 = plt.grid(True) # линии вспомогательной сетки
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/1c5d1f73-8ca5-4480-b039-39f037d87bce)
```
graph1 = plt.plot([-1.0, 1.0], [0.0, 1.0])
print('Plot: ', len(graph1), graph1)
text1 = plt.text(0.5, 0.5, 'Text on figure')
print('Text: ', type(text1))
grid1 = plt.grid(True) # линии вспомогательной сетки
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/ee38c016-6899-4a22-af2c-40e67021bb4d)

### 2. Диаграммы
```
import matplotlib.pyplot as plt
import numpy as np
s = ['one','two','three ','four' ,'five']
x = [1, 2, 3, 4, 5]
z = np.random.random(100)
z1 = [10, 17, 24, 16, 22]
z2 = [12, 14, 21, 13, 17]
fig = plt.figure()
plt.bar(x, z1)
plt.title('Simple bar chart')
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/6c453c37-7592-450e-9333-ce5961228ff0)
```
fig = plt.figure()
plt.hist(z)
plt.title('Simple histogramm')
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/0e05d703-3ff5-4f11-9f8a-347164154db9)
```
fig = plt.figure()
plt.pie(x, labels=s)
plt.title('Simple pie chart')
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/a9c784ba-e43f-42fa-80ea-ed61a200692d)

"Ящик с усами" (boxwhisker)
```
fig = plt.figure()
plt.boxplot([z1, z2])
plt.title('Simple box whisker chart')
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/9a628231-081b-4d3a-8d35-23ba58c2bd5d)

Оценка погрешности, "усы"
```
fig = plt.figure()
plt.errorbar(x, z1, xerr=1, yerr=0.5)
plt.title('Simple error bar chart')
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/94056547-bb1b-402f-9c9c-75ccb73d2014)

### 3. Изображения в изолиниях
```
import matplotlib.pyplot as plt
import numpy as np
# создаём матрицу значений
dat = np.random.random(200).reshape(20,10) 
fig = plt.figure()
# метод псевдографики pcolor
pc = plt.pcolor(dat) 
plt.colorbar(pc)
plt.title('Simple pcolor plot')
fig = plt.figure()
me = plt.imshow(dat) 
plt.colorbar(me)
plt.title('Simple imshow plot')
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/99a3085a-ad14-4f14-aef7-6bbc72b076ba)
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/9d2d7973-83eb-4b95-a6ad-21685e9ed0f4)
### 4. Методы отображения
```
import matplotlib.pyplot as plt
import numpy as np
# матрица значений
dat = np.random.random(200).reshape(20,10) 
fig = plt.figure()
cr = plt.contour(dat) 
plt.colorbar(cr)
plt.title('Simple contour plot')
fig = plt.figure()
cf = plt.contourf(dat) 
plt.colorbar(cf)
plt.title('Simple contourf plot')
fig = plt.figure()
cf = plt.matshow(dat) 
plt.colorbar(cf, shrink=0.7)
plt.title('Simple matshow plot')
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/94344364-5a69-4728-ae43-f9e61a4eb666)
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/3b65a80a-e1a1-46e2-abc5-46e55651a0a9)
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/dd72d803-c8c5-4851-a0ef-0f7f41167d5d)
### 5. Методы заливки
```
import matplotlib.pyplot as plt
import numpy as np
x = np.arange(0, 4*np.pi+0.1, 0.1)
y = np.sin(x)
z = np.sin(2*x)
x2 = np.arange(20)
y2 = -1.5*x2 + 2.33
z2 = 0.7*x2 - 8.5
```

Заливка многоугольника
```
fig = plt.figure()
# метод псевдографики pcolor
plt.fill(x, y, 'r') 
plt.title('Simple fill')
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/0578ead2-c4a0-46cd-89cc-0f3036149dd6)

Заливка между двумя линиями
```diff
# fill_between()
fig = plt.figure()
plt.plot(x2, z2, color='pink', linewidth=4.0)
plt.plot(x2, y2, color='r', linewidth=4.0)
plt.fill_between(x2, y2, z2) 
plt.title('Simple fill_between')
plt.grid(True)
# fill_betweenx()
fig = plt.figure()
plt.plot(z, x, color='pink', linewidth=4.0)
plt.plot(z, x-1.0, color='r', linewidth=4.0)
plt.fill_betweenx(z, x, x-1.0, color='cyan')
plt.title('Simple fill_betweenx')
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/53dbe0e0-5a1a-4bda-bdc0-61fbc3cbca4e)
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/4c90e622-fb37-4cb0-a283-8a3a16603093)

### 6. Векторные диаграммы
```
import matplotlib.pyplot as plt
import numpy as np
x = np.arange(-2*np.pi, 2*np.pi, 0.1)
u = np.sin(x)*np.cos(x)
v = np.cos(x)
uu, vv = np.meshgrid(u,v)
N = 100
x1 = np.random.random(N).reshape((10, 10))
y1 = np.random.random(N).reshape((10, 10))
```
Линии тока
```
fig = plt.figure()
plt.streamplot(x, x, uu, vv) 
plt.title('Simple stream plot')
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/23e38a8c-723c-4219-a4c3-dbc473b95b5c)

Векторное поле
```
fig = plt.figure()
plt.quiver(x1, y1, color='green') 
plt.title('Simple quiver plot')
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/9f13d4fd-2947-4b11-9643-f0a7777231da)

Многообразие и удобство создания графики в matplotlib обеспечивается за счёт созданных графических команд и богатого арсенала по
конфигурации типовых форм. Эта настройка включает в себя работу с цветом, формой, типом линии или маркера, толщиной линий,
степенью прозрачности элементов, размером и типом шрифта и другими свойствами.

Параметры, которые определяют эти свойства в различных графических командах, обычно имеют одинаковый синтаксис, то есть
называются одинаково. Стандартным способом задания свойств какого либо создаваемого объекта (или методу) является передача по
ключу: ключ = значение.

Наиболее часто встречаемые названия параметров изменения свойств графических объектов:
+ color/colors/c - цвет;
+ linewidth/linewidths - толщина линии;
+ linestyle - тип линии;
+ alpha - степень прозрачности (от полностью прозрачного 0 до непрозрачного 1);
+ fontsize - размер шрифта;
+ marker - тип маркера;
+ s - размер маркера в методе plt.scatter(только цифры);
+ rotation - поворот строки на X градусов.

## Различные формы области рисования
```
import matplotlib.pyplot as plt
```
Добавление на рисунок прямоугольной (по умолчанию) области рисования
```
fig = plt.figure()
ax = fig.add_axes([0, 0, 1, 1])
print (type(ax))
plt.scatter(1.0, 1.0)
plt.grid(True)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/300d1851-12dc-40fb-8747-a86233097b14)

Добавление на рисунок круговой области рисования
```
fig = plt.figure()
ax = fig.add_axes([0, 0, 1, 1], polar=True)
plt.scatter(0.0, 0.5)
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/9153cdef-1e7c-45f5-b1f3-cb08ba681683)

## Элементы рисунка Artists
Всё пространство рисунка Figure (прямоугольной или иной формы) можно использовать для нанесения других элементов рисунка,
например, контейнеров Axes, графических примитивов в виде линий, фигур, текста и так далее. В любом случае каждый рисунок можно
структурно представить следующим образом:
1.  Область рисования Axes
    + Заголовок области рисования -> plt.title();
2.  Ось абсцисс Xaxis
    + Подпись оси абсцисс OX -> plt.xlabel();
3. Ось ординат Yaxis
    + Подпись оси ординат OY -> plt.ylabel();
4. Легенда -> plt.legend();
5. Цветовая шкала -> plt.colorbar()
    + Подпись горизонтальной оси абсцисс OY -> cbar.ax.set_xlabel();
    + Подпись вертикальной оси абсцисс OY -> cbar.ax.set_ylabel();
7. Деления на оси абсцисс OX -> plt.xticks();
8. Деления на оси ординат OY -> plt.yticks().

Для каждого из перечисленных уровней-контейнеров есть возможность нанести заголовок (title) или подпись (label). Подписи к рисунку
облегчают понимание того, в каких единицах представлены данные на графике или диаграмме.

Также на рисунок можно нанести линии вспомогательной сетки (grid). В pyplot она вызывается командой plt.grid(). Вспомогательная сетка
связана с делениями координатных осей (ticks), которые определяются автоматически исходя из значений выборки.

В matplotlib существуют главные деления (major ticks) и вспомогательные (minor ticks) для каждой координатной оси. По умолчанию
рисуются только главные делений и связанные с ними линии сетки grid. В плане настройки главные деления ничем не отличаются от
вспомогательных.

## Координатные оси Axis
Данные, нанесённые тем или иным графическим способом на рисунок Figure и область рисования Axes, не представляют особого
интереса для научного анализа, пока они не привязаны к какой-либо системе координат. При создании экземпляра axes на вновь
созданной области рисования автоматически задаётся прямоугольная система координат, если не указаны атрибуты polar или projection.

Система координат определяет вид координатных осей. По умолчанию задаётся прямоугольная система координат: ось абсцисс (ось "OX")
и ось ординат (ось "OY"). В полярной системе координат координатными осями являются радиус и угол наклона, что выражается в виде
своеобразного тригонометрического круга.

Для хранения и форматирования каждой координатной оси и существует контейнер Axis.

## Контейнер Axis
Axis (не путать с областями рисования Axes) - контейнер в matplotlib, который привязан к области рисования Axes и на котором
располагаются деления осей (ticks), подписи делений (tick labels) и подписи осей (axis labels). Это третья "матрёшка" после Figure и Axes.
Координатные оси являются экземплярами класса matplotlib.axis.Axis.

Любая область рисования Axes содержит два особых Artist-контейнера: XAxis и YAxis. 

Они отвечают за отрисовку делений (ticks) и
подписей (labels) координатных осей, которые хранятся как экземпляры в переменных xaxis и yaxis. Чтобы обратиться к экземпляру axis,
отвечающему, например, за ось ординат, нужно обратиться к контейнеру yaxis соответствующей области рисования ax. Причём запись
"yax=ax.get_yaxis()" идентична записи "yax=ax.yaxis".

Каждый экземпляр axis содержит атрибут подписей (label) координатной оси и список главных (major ticks) и вспомогательных (minor ticks)
делений, а также является хранилищем для экземпляров делений (ticks).

Деления - это экземпляры класса matplotlib.axis.Tick, которые визуализируют деления (размер, цвет, толщину и т.д.) и подписи к ним.
Деления создаются динамически исходя из области изменения переданных данных. В результате на координатной оси появляются и
хранятся экземпляры классов matplotlib.axis.XTick и matplotlib.axis.YTick. Они родственны классу matplotlib.axis.Tick.

Хотя все графические примитивы, с помощью которых создаётся облик координатной оси содержатся в делениях ticks, у экземпляров axis
есть средства для управления линиями делений (tick lines), подписями делений (tick labels), а также местоположением делений (tick
locations).
```
import matplotlib.pyplot as plt
import numpy as np
fig = plt.figure()
ax = fig.add_subplot(111)
x = np.arange(100)
y = -np.exp(-0.3*x) + 2.33
ax.plot(x, y, 'k')
xax = ax.xaxis
xlocs = xax.get_ticklocs()
print ('Major X-ticks locations:', xlocs)
xlabels = xax.get_ticklabels()
print ('Major X-ticks labels:', xlabels)
xlines = xax.get_ticklines()
print ('Major X-ticks tick lines:', xlines)
# Линии вспомогательной сетки (главные деления) только по оси абсцисс
xax.grid(True)
for label in xlabels:
 # цвет подписи деленений оси OX
 label.set_color('blue')
 # поворот подписей деленений оси OX 
 label.set_rotation(45)
 # размер шрифта подписей делений оси OX 
 label.set_fontsize(14)
 
plt.show()
```
![image](https://github.com/motorollainlinux/26.10.2023/assets/112952627/3c0aa9a2-8be0-4a06-a635-6f54dc6de889)

## Главные и вспомогательные деления
В matplotlib деления на координатной оси могут быть главными (major ticks) или вспомогательными (minor ticks). По умолчанию наносятся
главные деления, к ним привязывается нанесение вспомогательной сетки grid, за которую отвечает параметр axes.grid.which': u'major' из
rcParams.

Работа со вспомогательными делениями ничем не отличается от работы с главными делениями: используются те же методы, но
указывается тип делений через определённый атрибут. Обычно этот атрибут называется which (принимает значения 'major'/'minor') или
булевый атрибут minor.

Ниже представлен список некоторых методов Axis. Они применяются к экземплярам xaxis или yaxis. Так как при работе с атрибутами
поддерживается идеология "set-get", то списке указаны методы только с приставкой "set_". Для получения значений атрибутов какой-либо
характеристики, нужно в методе заменить приставку "set" на "get": set_scale -> get_scale; get_data_interval -> set_data_interval и так далее.

Вспомогательный метод -> Описание
+ set_scale -> определяет характер изменений значений на оси: линейный 'linear' (по умолчанию) или 'log' логарифмический;
+ set_view_interval -> экземпляр интервала области изменения отображений по оси axis;
+ set_data_interval -> экземпляр интервала области изменения данных по оси axis;
+ set_gridlines -> список линий сетки для Axis;
+ set_label -> подпись оси, экземпляр Text;
+ set_ticklabels -> подписи делений, список экземпляров Text. Тип делений определяет параметр minor=True/False;
+ set_ticklines -> конфигурация делений, список экземпляров Line2D. Тип делений определяет параметр minor=True/False;
+ set_ticklocs -> положение делений. Тип делений определяет параметр minor=True/False;
+ set_major_locator -> экземпляр matplotlib.ticker.Locator для главных (major) делений;
+ set_major_formatter -> экземпляр matplotlib.ticker.Formatter для главных делений;
+ set_major_ticks -> список экземпляров Tick для главных делений;
+ set_minor_locator -> экземпляр matplotlib.ticker.Locator для вспомгательных (minor) делений;
+ set_minor_formatter -> экземпляр matplotlib.ticker.Formatter для вспомогательных делений;
+ set_minor_ticks -> список экземпляров Tick для вспомогательных делений;
+ grid -> определяет будет ли отображаться линии сетки для главной или вспомогательных делений (параметр which='major'/'minor');
