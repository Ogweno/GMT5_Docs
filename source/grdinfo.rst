.. index:: ! grdinfo

grdinfo
=======

官方文档： :ref:`gmt:grdinfo`

grdinfo用于从2D网格文件中提取基本信息。

最小示例
--------

grdinfo后直接跟网格文件，会显示网格文件的信息::

    $ gmt grdinfo test1.nc
    test1.nc: Title: ETOPO5 global topography
    test1.nc: Command: grdraster -R100/150/-30/30 1 -Gtest.nc
    test1.nc: Remark: /opt/GMT-4.5.13/share/dbase/etopo5.i2
    test1.nc: Gridline node registration used [Geographic grid]
    test1.nc: Grid file format: nf = GMT netCDF format (32-bit float), COARDS, CF-1.5
    test1.nc: x_min: 100 x_max: 150 x_inc: 0.0833333333333 name: longitude [degrees_east] nx: 601
    test1.nc: y_min: -30 y_max: 30 y_inc: 0.0833333333333 name: latitude [degrees_north] ny: 721
    test1.nc: z_min: -10376 z_max: 6096 name: m
    test1.nc: scale_factor: 1 add_offset: 0
    test1.nc: format: classic

从输出中可以看到很多信息：

- 网格文件中的标题信息；
- 生成该网格文件的命令；
- 网格文件的配准方式，此处为Gridline配准；
- 数据格式为netCDF 32位float；
- 数据中X维度的最小值x_min、最大值x_max、网格间隔x_inc以及数据点数nx；
- 数据中Y维度的最小值y_min、最大值y_max、网格间隔y_inc以及数据点数ny；
- 数据中Z值的最小值z_min和最大值z_max以及其他信息；

F选项
-----

以另一种输出格式显示上例中的输出信息。

C选项
-----

将输出以Tab分隔显示在一行中。输出格式为::

    name w e s n z0 z1 dx dy nx ny [x0 y0 x1 y1] [med scale] [mean std rms] [n_nan]

默认只输出前11列，其含义看名字就可以猜到了。若使用了-M、-L1、-L2、-M选项，则会输出对应的方括号内的项。

若使用了-I选项，则输出格式为::

    NF w e s n z0 z1

其中NF是读入的网格文件数目。

::

    $ gmt grdinfo test1.nc -C
    test1.nc    100 150 -30 30  -10376  6096    0.0833333333333 0.0833333333333 601 721

I选项
-----

- -I后不接任何参数，会以\ **-I**\ *xinc/yinc*\ 的格式输出网格间隔
- ``-I-``\ 会以\ **-R**\ *w/e/s/n*\ 的格式输出网格范围
- ``-Ib``\ 会列出网格区域范围的四个顶点对应的坐标
- **-I**\ *dx/dy*\ 会先获取网格的区域范围，并对该范围做微调使得其是dx和dy的整数倍，并以\ **-R**\ *w/e/s/n*\ 的格式输出

::

     $ gmt grdinfo test1.nc -I
     -I0.0833333333333/0.0833333333333
     $ gmt  gmt grdinfo test1.nc -I-
     -R100/150/-30/30
     $ gmt  gmt grdinfo test1.nc -Ib
     > Bounding box for test1.nc
     100    -30
     150    -30
     150    30
     100    30
     $ gmt  gmt grdinfo test1.nc -I3/3
     -R99/150/-30/30

当\ **-I**\ *dx/dy*\ 与-C选项连用时::

    $ gmt grdinfo test1.nc -I3/3 -C
    1   99  150 -30 30  -10376  6096

L选项
-----

- ``-L0``: 扫描整个数据并报告Z值的范围，而不仅仅只是从网格的头段中读取
- ``-L1``: 输出中位数以及L1 scale(L1 scale= 1.4826\*Median Absolute Deviation)
- ``-L2``: 输出均值、标准差以及均方根

M选项
-----

寻找并报告Z最小和最大值所对应的坐标，以及值为NaN的网格点的数目

R选项
-----

从网格文件中取出一个子区域，并报告该子区域的信息

T选项
-----

- **-T**\ *dz*: 提取Z的最小最大值，并做微调使得最值是dz的整数倍，然后以\ ``-Tzmin/zmax/dz``\ 的格式输出。
- **-Ts**\ *dz*: 与上面类似，唯一的区别在于会根据Z的绝对值最大值，输出一个关于0对应的范围。

::

    $ gmt grdinfo test1.nc -T0.1
    -T-10376/6096/0.1
    $ gmt grdinfo test1.nc -Ts0.1
    -T-10376/10376/0.1
