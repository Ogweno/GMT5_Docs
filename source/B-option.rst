B选项
=====

B选项用于控制底图边框的显示效果。

对于整个底图边框和每对坐标轴，B选项拥有不同的语法，所以在一个命令中可能会多次出现B选项。

当命令中没有出现B选项，则默认不绘制底图边框。

边框设置
--------
B选项对于边框设置有一套专门的语法：

**-B**\ [*axes*][**+b**][**+g**\ *fill*][**+o**\ *lon/lat*][**+t**\ *title*]

其中：

- ``axes``\ 用于控制显示哪些坐标轴；
- ``+b``\ 用于在3D绘图中根据R选项指定的范围绘制长方体的12条边；
- ``+g``\ 用于在底图内部填色；
- ``+o``\ 用于指定网格线的参考点；
- ``+t``\ 用于指定当前底图的标题；

通常情况下，只需要使用axes和+t选项。

axes
~~~~

*axes*\ 控制要绘制哪些轴以及这些轴是否显示标注。对于一般的二维图而言，有上下左右四条轴，这四条轴分别用四个方向的英文单词首字母表示，即上北（N）下南（S）左西（W）右东（E）。对于每条轴都有三种状态：

#. 不出现该字母表示不绘制这条边
#. 用大写字母表示绘制这条边，且该边有刻度、有标注
#. 用小写字母表示绘制这条边，但该边有刻度、无标注

下面给出一些具体的示例，解释一下\ *axes*\ 取不同值的效果：

#. ``WSEN``\ ：绘制四条边，且每条边都有刻度和标注
#. ``WSn``\ ：绘制左、下、上三条边，不绘制右边。其中左边和下边有刻度和标注，上边无标注；

下面两个命令，分别使用了不同的B选项，可以自己执行，查看绘图效果并试着理解axes的用法::

    gmt psbasemap -R0/10/0/10 -JX5c -B2 -BWSEN > test1.ps
    gmt psbasemap -R0/10/0/10 -JX5c -B2 -BWSn > test2.ps

对于3D绘图来说，axes还可以加上一个\ ``Z``\ 。同理，大写的Z表示有刻度和标注，小写的z表示无标注。默认情况下，只会绘制一条垂直的边，可以使用\ ``1234``\ 的任意组合来表示要绘制哪些垂直边。其中1表示左下角的垂直边，其他垂直边按逆时针顺序编号。加上\ ``+b``\ 子选项，会绘制一个由R选项范围决定的长方体的12条边，即相当于一个box。如果Z轴有指定网格间距，则会在xz和yz平面内显示网格线。

下面的命令展示了3D绘图中B选项的不同用法，读者可以自己一一测试，根据绘图效果理解B选项中各字母的含义。命令中的某些选项还没有介绍过，暂时可以不必理会其含义::

    gmt psbasemap -R0/10/0/10/0/10 -JX5c -JZ5c -Bz2 -BWSENZ -p45/45 > test1.ps
    gmt psbasemap -R0/10/0/10/0/10 -JX5c -JZ5c -Bz2 -BWSENZ1234 -p45/45 > test2.ps
    gmt psbasemap -R0/10/0/10/0/10 -JX5c -JZ5c -Bz2 -BWSEN+b -p45/45 > test3.ps
    gmt psbasemap -R0/10/0/10/0/10 -JX5c -JZ5c -Bz2 -B+b -p45/45 > test4.ps
    gmt psbasemap -R0/10/0/10/0/10 -JX5c -JZ5c -Bz2 -B+b -p45/45 > test5.ps
    gmt psbasemap -R0/10/0/10/0/10 -JX5c -JZ5c -B2 -Bz2 -BwSEnZ+b -p45/45 > test6.ps

+g
~~

用于对边框内部进行填充，默认情况下是不填充的。关于颜色填充，参考\ :doc:`fill`\ 一节。

::

    gmt psbasemap -R0/10/0/10 -JX5c -B2 -BWSEN+glightblue > test.ps

+o
~~

默认情况下，网格线是以北极点作为参考的，如果你想要以另一个点作为参考绘制倾斜的网格线，则可以使用\ ``+o``\ 子选项。

+t
~~

``+t``\ 用于给当前底图加标题，该标题位于底图的上方中部。标题可以是任意字符串，如果是字符串中有空格，则必须用引号将字符串括起来（没有空格的时候也可以括起来）。

::

    gmt psbasemap -R0/10/0/10 -JX5c -Ba2g2 -BWSEN+glightblue+ttitle > test.ps
    gmt psbasemap -R0/10/0/10 -JX5c -Ba2g2 -BWSEN+glightblue+t"This is title" > test2.ps

轴设置
------

X轴、Y轴、Z轴，每条轴都有很多属性，包括刻度间隔、网格线间隔、轴标签以及标注的间隔、前缀和单位。轴属性可以用如下语法控制：

**-B**\ [**p**\|\ **s**][**x**\|\ **y**\|\ **z**]\ *intervals*\ [\ **+l**\ *label*][**+p**\ *prefix*][**+u**\ *unit*]

为了更加清晰，以上的语法也可以被分为如下两句：

- **-B**\ [**p**\|\ **s**][**x**\|\ **y**\|\ **z**][**+l**\ *label*][**+p**\ *prefix*][**+u**\ *unit*]
- **-B**\ [**p**\|\ **s**][**x**\|\ **y**\|\ **z**]\ *intervals*

p|s
~~~

对于每个轴来说，都有两个等级的属性可以设置，分别称为p（Primary）和s（Secondary）。

对于地理坐标而言，通常只需要使用默认的Primary属性即可。而Secondary则主要用于坐标轴为时间轴的情况下，此时p和s分别用于指定不同尺度的时间间隔。在GMT默认的情况下，p属性的标注比较靠近坐标轴，而s属性的标注离坐标轴稍远。p和s的用法与区别，可以参考后面给出的例子。

x|y|z
~~~~~

要设置哪些边的信息，默认值为xy，即同时设置X轴和Y轴的信息。可以指定单个轴（比如只有x），也可以同时指定多个轴（比如xy和xyz）。如果想要不同轴有不同的设置，则需要多次使用-B选项，每个指定不同的轴。

子选项
~~~~~~

- ``+l``\ ：给选中的轴加标签；
- ``+p``\ ：给选中的轴的标注加前缀；
- ``+u``\ ：给选中的轴的标注加单位。对于地图而言，标注的单位为度，该符号是自动添加的，由\ :ref:`FORMAT_GEO_MAP <FORMAT_GEO_MAP>`\ 控制。

interval
~~~~~~~~

每个轴都有三个属性，分别是标注（annotation）、刻度（frame）和网格线（grid）。下图展示了这三个名词在绘图时的具体含义。

.. figure:: /images/GMT_-B_afg.*
   :width: 500px
   :align: center

*interval*\ 可以用于设置这三个属性的间隔，它是一个或多个[**t**]\ *stride*\ [+-*phase*][**u**]的组合。

- **t**\ 可以取a（标注）、f（刻度）、g（网格线），表明了要设置轴的哪部分的间隔
- **stride**\ 用于设置间隔，stride为0，表示不绘制
- **phase**\ 可以用于控制标注、刻度或网格线的起算点
- **u**\ 是间隔的单位

比如：\ ``-Ba30f15g15``\ ，\ ``-Bxa10 -Bya15``\ 等等。

B选项还有一个可以自动计算间隔的功能，\ ``-Bafg``\ 会根据当前的区域大小等信息自动计算合适的间隔，\ ``-Bafg/afg``\ 则会对X轴和Y轴分别计算合适的间隔。

地理底图
--------

地理底图与一般的坐标轴不同，其底图类型\ :ref:`MAP_FRAME_TYPE`\ 使用\ ``fancy``\ 形式。

.. _basemap_border:

.. figure:: /images/GMT_-B_geo_1.*
   :width: 500 px
   :align: center

   地理底图

下图同时使用了p和s两级属性。这里p属性用于显示弧度，s属性用于显示弧分。

.. _complex_basemap:

.. figure:: /images/GMT_-B_geo_2.*
   :width: 500 px
   :align: center

   同时使用P和S两级属性的地理底图

笛卡尔线性轴
------------

对于一般的线性轴而言，标注的格式由\ :ref:`FORMAT_FLOAT_OUT <FORMAT_FLOAT_OUT>`\ 决定，其默认值为\ ``%g``\ ，即根据数据的大小决定用一般表示还是指数表示，小数位的数目会根据\ *stride*\ 自动决定。若设置\ :ref:`FORMAT_FLOAT_OUT <FORMAT_FLOAT_OUT>`\ 为其他值，则会严格使用其定义的格式，比如\ ``%.2f``\ 表示显示两位小数。

.. _axis_label_basemap:

.. figure:: /images/GMT_-B_linear.*
   :width: 500 px
   :align: center

   笛卡尔线性轴。
   ``-R0/12/0/0.95 -JX3i/0.3i -Ba4f2g1+lFrequency+u" %" -BS``

笛卡尔log\ :sub:`10`\ 轴
------------------------

由于对数坐标的特殊性，\ *stride*\ 参数具有特殊的含义。下面说明\ *stride*\ 在对数坐标下的特殊性：

- *stride*\ 必须是1、2、3或负整数-n。

  - ``1``\ ：每10的指数；
  - ``2``\ ：每10的指数的1、2、5倍；
  - ``3``\ ：每10的指数的0.1倍；
  - ``-n``\ ：每10的n次方出现一次；

- 在\ *stride*\ 后加\ ``l``\ ，则标注会以log\ :sub:`10`\ 的值显示，比如100会显示成2.
- 在\ *stride*\ 后加\ ``p``\ ，则标注会以10的n次方的形式显示，比如10\ :sup:`-5`

.. _Log_projection:

.. figure:: /images/GMT_-B_log.*
   :width: 500 px
   :align: center

   对数坐标轴。
   (上) \ ``-R1/1000/0/1 -JX3il/0.25i -Ba1f2g3``\
   (中) \ ``-R1/1000/0/1 -JX3il/0.25i -Ba1f2g3l``\
   (下) \ ``-R1/1000/0/1 -JX3il/0.25i -Ba1f2g3p``\

笛卡尔指数轴
------------

正常情况下，\ *stride* \ 用于生成等间隔的标注或刻度，但是由于指数函数的特性，这样的标注会在坐标轴的某一端挤在一起。为了避免这个问题，可以在\ *stride*\ 后加\ ``p``\ ，则标注会按照转换后的值等间隔出现，而标注本身依然使用未转换的值。比如，若stride=1，pow=0.5（即sqrt），则在1、4、处会出现标注。

.. _Pow_projection:

.. figure:: /images/GMT_-B_pow.*
   :width: 500 px
   :align: center

   指数投影坐标轴。
   (上) ``-R0/100/0/0.9 -JX3ip0.5/0.25i -Ba20f10g5``
   (下) ``-R0/100/0/0.9 -JX3ip0.5/0.25i -Ba3f2g1p``

时间轴
------

时间轴与其他轴不同的地方在于，时间轴可以有多种不同的标注方式。下面会用一系列示例来演示时间轴的灵活性。在下面的例子中，尽管只绘制了X轴（绘图时使用了-BS），实际上时间轴标注的各种用法使用于全部轴。

第一个例子展示了2000年春天的两个月，想要将这两个月的每周的第一天的日期标注出来::

     gmt set FORMAT_DATE_MAP=-o FONT_ANNOT_PRIMARY +9p
     gmt psbasemap -R2000-4-1T/2000-5-25T/0/1 -JX5i/0.2i -Bpa7Rf1d -Bsa1O -BS -P > GMT_-B_time1.ps

绘图效果如图\ :ref:`Cartesian time axis <cartesian_axis1>`\ 所示，需要注意\ :ref:`FORMAT_DATE_MAP <FORMAT_DATE_MAP>`\ 前的破折号会去掉日期前面的前置零（即02变成2）。

.. _cartesian_axis1:

.. figure:: /images/GMT_-B_time1.*
   :width: 500 px
   :align: center

   时间轴示例1

下面的例子用两种不同的方式标注了1969年的两天::

     gmt set FORMAT_DATE_MAP "o dd" FORMAT_CLOCK_MAP hh:mm FONT_ANNOT_PRIMARY +9p
     gmt psbasemap -R1969-7-21T/1969-7-23T/0/1 -JX5i/0.2i -Bpa6Hf1h -Bsa1K -BS -P -K > GMT_-B_time2.ps
     gmt psbasemap -R -J -Bpa6Hf1h -Bsa1D -BS -O -Y0.65i >> GMT_-B_time2.ps

绘图效果如图\ :ref:`cartesian_axis2`\ 所示。图中下面的例子使用周来标注，上面的例子使用日期来标注。

.. _cartesian_axis2:

.. figure:: /images/GMT_-B_time2.*
   :width: 500 px
   :align: center

   时间轴示例2

第三个例子展示了两年的时间，并标注了每年以及每三个月::

     gmt set FORMAT_DATE_MAP o FORMAT_TIME_PRIMARY_MAP Character FONT_ANNOT_PRIMARY +9p
     gmt psbasemap -R1997T/1999T/0/1 -JX5i/0.2i -Bpa3Of1o -Bsa1Y -BS -P > GMT_-B_time3.ps

年标注位于一年间隔的中间，月标注位于对应月的中间而不是三个月间隔的中间。

.. _cartesian_axis3:

.. figure:: /images/GMT_-B_time3.*
   :width: 500 px
   :align: center

   时间示例3

第四个例子展示了一天中的几个小时，通过在R选项中指定\ ``t``\ 来使用相对时间坐标。这里使用了p属性和s属性，12小时制，时间从右向左增加::

     gmt set FORMAT_CLOCK_MAP=-hham FONT_ANNOT_PRIMARY +9p
     gmt psbasemap -R0.2t/0.35t/0/1 -JX-5i/0.2i -Bpa15mf5m -Bsa1H -BS -P > GMT_-B_time4.ps

.. _cartesian_axis4:

.. figure:: /images/GMT_-B_time4.*
   :width: 500 px
   :align: center

   时间轴示例4

第五个例子用两种方式展示了几周的时间::

    gmt set FORMAT_DATE_MAP u FORMAT_TIME_PRIMARY_MAP Character \
           FORMAT_TIME_SECONDARY_MAP full FONT_ANNOT_PRIMARY +9p
    gmt psbasemap -R1969-7-21T/1969-8-9T/0/1 -JX5i/0.2i -Bpa1K -Bsa1U -BS -P -K > GMT_-B_time5.ps
    gmt set FORMAT_DATE_MAP o TIME_WEEK_START Sunday FORMAT_TIME_SECONDARY_MAP Chararacter
    gmt psbasemap -R -J -Bpa3Kf1k -Bsa1r -BS -O -Y0.65i >> GMT_-B_time5.ps

.. _cartesian_axis5:

.. figure:: /images/GMT_-B_time5.*
   :width: 500 px
   :align: center

   时间轴示例5

第六个例子展示了1996年的前5个月，每个月用月份的简写以及两位年份标注::

    gmt set FORMAT_DATE_MAP "o yy" FORMAT_TIME_PRIMARY_MAP Abbreviated
    gmt psbasemap -R1996T/1996-6T/0/1 -JX5i/0.2i -Ba1Of1d -BS -P > GMT_-B_time6.ps

.. _cartesian_axis6:

.. figure:: /images/GMT_-B_time6.*
   :width: 500 px
   :align: center

   时间轴示例6

第七个例子::

    gmt set FORMAT_DATE_MAP jjj TIME_INTERVAL_FRACTION 0.05 FONT_ANNOT_PRIMARY +9p
    gmt psbasemap -R2000-12-15T/2001-1-15T/0/1 -JX5i/0.2i -Bpa5Df1d -Bsa1Y -BS -P > GMT_-B_time7.ps

.. _cartesian_axis7:

.. figure:: /images/GMT_-B_time7.*
   :width: 500 px
   :align: center

   时间轴示例7

自定义轴
--------

GMT允许用户定义标注来实现不规则间隔的标注，用法是\ ``-Bc``\ 后接标注文件名。

标注文件中以“#”开头的行为注释行，其余为记录行，记录行的格式为::

    *coord* *type* [*label*]

- *coord*\ 是需要标注、刻度或网格线的位置；
- *type*\ 是如下几个字符的组合

  - ``a``\ 或\ ``i``\ 前者为annotation，后者表示interval annotation
  - 在一个标注文件中，\ ``a``\ 和\ ``i``\ 只能出现其中的任意一个
  - ``f``\ 表示刻度，即frame tick
  - ``g``\ 表示网格线，即gridline

- *label* \ 默认的标注为\ *coord*\ 的值，若指定label，则使用label的值

需要注意，coord必须按递增顺序排列。

下面的例子展示中展示了自定义标注的用法，xannots.txt和yannots.txt分别是X轴和Y轴的标注文件。

::

    cat << EOF > xannots.txt
    416.0 ig Devonian
    443.7 ig Silurian
    488.3 ig Ordovician
    542 ig Cambrian
    EOF
    cat << EOF > yannots.txt
    0 a
    1 a
    2 f
    2.71828 ag e
    3 f
    3.1415926 ag @~p@~
    4 f
    5 f
    6 f
    6.2831852 ag 2@~p@~
    EOF
    gmt psbasemap -R416/542/0/6.2831852 -JX-5i/2.5i -Bpx25f5g25+u" Ma" -Bpycyannots.txt \
                  -BWS+glightblue -P -K > GMT_-B_custom.ps
    gmt psbasemap -R416/542/0/6.2831852 -JX-5i/2.5i -Bsxcxannots.txt -BWS -O \
                  --MAP_ANNOT_OFFSET_SECONDARY=10p --MAP_GRID_PEN_SECONDARY=2p >> GMT_-B_custom.ps
    rm -f [xy]annots.txt

.. _Custom_annotations:

.. figure:: /images/GMT_-B_custom.*
   :width: 500 px
   :align: center

   自定义坐标轴
