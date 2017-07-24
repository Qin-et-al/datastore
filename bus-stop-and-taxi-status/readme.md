#### 魔都公交站与出租车载客/空车时空分布数据说明

##### 0. 数据下载

| 类别                                       | 下载（或右键另存为）                               | 备注        |
| ---------------------------------------- | ---------------------------------------- | --------- |
| [2.1 全上海公交站点经纬度](#user-content-21-全上海公交站点经纬度) | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/2017-bus-stopnames-lonlat-lines-sorted.xlsx) | 站名-经纬度-线路 |
| [2.2 外环内公交站点数量统计](#user-content-22-外环内公交站点数量统计) | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/2017-bus-stops-count-in-outer-ring-shanghai.csv) |           |
| [2.3 载客/空车出租车数量统计](#user-content-23-载客空车出租车数量统计) | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-210000-status-1-in-outer-ring-shanghai.csv) | 载客21:00数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-213000-status-1-in-outer-ring-shanghai.csv) | 载客21:30数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-220000-status-1-in-outer-ring-shanghai.csv) | 载客22:00数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-223000-status-1-in-outer-ring-shanghai.csv) | 载客22:30数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-230000-status-1-in-outer-ring-shanghai.csv) | 载客23:00数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-233000-status-1-in-outer-ring-shanghai.csv) | 载客23:30数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-210000-status-0-in-outer-ring-shanghai.csv) | 空车21:00数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-213000-status-0-in-outer-ring-shanghai.csv) | 空车21:30数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-220000-status-0-in-outer-ring-shanghai.csv) | 空车22:00数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-223000-status-0-in-outer-ring-shanghai.csv) | 空车22:30数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-230000-status-0-in-outer-ring-shanghai.csv) | 空车23:00数据 |
|                                          | [下载](https://github.com/getAbchin/datastore/raw/master/bus-stop-and-taxi-status/data/20110406-233000-status-0-in-outer-ring-shanghai.csv) | 空车23:30数据 |

##### 1. 空间范围

以外环高速**最小矩形包络面**为空间边界，具体经纬度限界为：

```python
# Outer ring road boundary
# CRS: WGS-84
west_long = 121.351
east_long = 121.658
north_lat = 31.380
south_lat = 31.104
```

###### 1.1 网格化尺度

该空间按照0.01°间隔划分为方形网格区域（在较小尺度下，0.01°≈100m），相当于网格尺度为100m×100m。这样，矩形包络面被划分为277（南北）×308（东西）的网格空间。

###### 1.2 网格划分方式

经纬度一般保留到小数点后6位，对于这样精度的经纬度要实现0.01°间隔的划分，只需要将该经纬度四舍五入到0.01°，并用该精度的经纬度作为网格编号。

> 例如，网格(121.440, 31.120)​表示东西跨越​(121.439500, 121.440499)​且南北跨越​(31.119500, 31.120499)​范围的网格（这些范围内的点均将四舍五入至(121.440, 31.120)​），该网格尺度相当于100m×100m。

##### 2. 数据格式

###### 2.1 全上海公交站点经纬度

包含32371条站点的序号、站名、经度、纬度和经过线路5个字段，涉及1.4万个公交站名，每个公交站名一般会对应2个以上（上下游各设一站）实体站点。外环内约有5千多个站名，对应了1万多个站点。

###### 2.2 外环内公交站点数量统计

按照[1.2 网格划分方式](#user-content-12-网格划分方式)，将[2.1 全上海公交站点经纬度](#user-content-21-全上海公交站点经纬度)的经纬度信息按网格分组统计站点数量，csv数据第一行和第一列分别为经度和纬度，第二行第二列起为站点数量。

该表格的`imshow()`图（如下）直观显了外环内公交站点的空间分布特征。

![](https://github.com/getAbchin/datastore/blob/master/bus-stop-and-taxi-status/img/bus-stop-distribution.png?raw=true)

###### 2.3 载客/空车出租车数量统计

选取了2011年4月6日，外环矩形包络面以内的出租车经纬度信息。为了真实反映夜间公交车收班后的供需关系，特别抽取了21:00至23:30每半小时的截面载客/空车车辆数。

抽取方法如下，以时间截面21:30为例，选取21:29:30至21:30:30这1分钟之间的所有出租车经纬度与载客标识（0或1），对每一个出租车司机，计算这1分钟内，TA的平均经度、平均纬度、平均状态（小于0.5认为空车，反之认为载客）；然后，按照[1.2 网格划分方式](#user-content-12-网格划分方式)分别对载客状态和空车状态统计各网格的车辆数；最终得到[0. 数据下载](#user-content-0-数据下载)中提供的12个表格（载客/空车各6各时间截面），格式与[2.2 外环内公交站点数量统计](#user-content-22-外环内公交站点数量统计)一致。

> 12个表格记录了供需关系的事变特征，**可按需**选择某一组时间截面的载客/空车表格做进一步分析。

12个表格的`imshow()`图（如下）直观显示了空车和载客出租车在时空上分布的走势（[点击](https://github.com/getAbchin/datastore/blob/master/bus-stop-and-taxi-status/img/taxi-status-distribution.png?raw=true)查看大图）。

![](https://github.com/getAbchin/datastore/blob/master/bus-stop-and-taxi-status/img/taxi-status-distribution.png?raw=true)

注：以上经纬度坐标系均为 [WGS84](https://en.wikipedia.org/wiki/World_Geodetic_System#A_new_World_Geodetic_System:_WGS_84)。

##### 3. 魔都公交站空间数据来源

Data source: [aibang.com](http://www.aibang.com/) API doc: [here](http://www.aibang.com/api/usage#bus_stats_xy) (Chinese).

I wrote several rows of scraping code in python to call the API, it returned **bus stop names** and **lon-lats** near given lon-lat within a specific distance (10km at most, if that could be set 100km, the following text could be omitted). Iterating meshgrid lon-lats (evenly distributed) in the boundary of Shanghai, I obtained almost all bus stop names and locations.

Lon-lats from the source is using Chinese 'Martian' Coordinate Reference System (CRS) [gcj02](https://en.wikipedia.org/wiki/Restrictions_on_geographic_data_in_China#GCJ-02), I need convert it to the universal CRS [WGS84](https://en.wikipedia.org/wiki/World_Geodetic_System#A_new_World_Geodetic_System:_WGS_84). (this [github gist](https://gist.github.com/jp1017/71bd0976287ce163c11a7cb963b04dd8) helped me do the trick).

Converted lon-lats can be now projected on the map. I did that on qgis with [OpenStreetMap](http://www.openstreetmap.org/) plugin providing the background map layer, and I set the points displayed as a heatmap, the final image was produced.

![](https://github.com/getAbchin/datastore/blob/master/bus-stop-and-taxi-status/img/bus-stop-location-heatmap.png?raw=true)

