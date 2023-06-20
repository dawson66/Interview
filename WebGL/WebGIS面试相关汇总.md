## 常见的地图框架有哪些？

* 2D
  * Openlayers
  * Leaflet
  * Mapbox GL JS
  * ArcGIS API for JS
  * 百度地图 JS API
  * 高德地图 JS API

* 3D
  * Three.js
  * Cesium.js



## 常见的数据类型

[常见的数据类型](https://blog.csdn.net/Zsh00058555/article/details/114639239)

1. OSGB ：倾斜摄影三维模型数据的组织方式，一般为二进制存储，带有纹理数据（jpg）；特点：数据文件碎、数量多、高级别金字塔文件大等，难以形成高效、标准的网络发布方案，一般使用需要转换。
2. OBJ：标准的3D模型文件格式，很适合在3D模型软件之间互导。
3. LAS/LAZ：激光扫描点云数据。LAZ为LAS的无损压缩版本。
4. FBX：由Autodesk开发的格式，支持动画、骨骼、贴图等多种属性；FBX最大的用途是用在诸如在3dsMax、Maya、softimage等软件间进行模型、材质、动作和摄影机信息的互导，这样就可以发挥max和maya等软件的优势。可以说，FBX方案是最好的互导方案。
5. 3ds：3ds是3dsmax建模软件的衍生文件格式，做完MAX的场景文件后可导出成3ds格式，可与其他建模软件兼容，也可用于渲染。
6. STL：一种简单的三角形网格模型格式，常用于3D打印领域。
7. DAE
8. DWF：

## 常见坐标系

1. **WGS84坐标系**，熟称地理坐标系/大地坐标系：EPSG：4326，也是通常说的经纬度坐标，单位为度。

2. **WGS84 Web墨卡托**：

   代号EPSG：3857，主流的web地图都使用Web墨卡托，如国外的Google Maps、OpenstreetMap、Bing Map、ArcGis等。国内的百度地图、高德地图、腾讯地图和天地图等也是基于墨卡托进行加密的；一般有两种情况：

   1. 在Web墨卡托的基础上，经过国家标准加密的 **国标02坐标系**，熟称 **火星坐标系**。
   2. 另一种，是在国标02坐标系下进一步进行加密，如百度地图的BD09坐标系。

3. **GCJ02经纬度投影**： 

   GCJ-02是由中国国家测绘局（G表示Guojia国家，C表示Cehui测绘，J表示Ju局）制订的地理信息系统的坐标系统。

   它其实就是对真实坐标系统进行人为的加偏处理，按照特殊的算法，将真实的坐标加密成虚假的坐标，而这个加偏并不是线性的加偏，所以各地的偏移情况都会有所不同。而加密后的坐标也常被大家称为“火星坐标系统”。

   该坐标系的坐标值为`经纬度格式`，单位为度。

   这里的GCJ02经纬度投影，也就是在WGS84经纬度的基础之上，进行GCJ-02加偏。

4. **GCJ02 Web 墨卡托投影**：同上，只不过为墨卡托投影，该坐标系的坐标值为web墨卡托格式，单位为米。

5. **BD09经纬度投影**：

   BD09 Web 墨卡托属于百度坐标系，它是在标准Web墨卡托的基础上进行GCJ-02加偏之后，再加上百度自身的加偏算法，也就是在Web墨卡托的基础之上进行了两次加偏。 该坐标系的坐标值为`Web墨卡托格式`，单位为米。

6. **CGCS2000坐标系**：

   代号**EPSG：4526**，2000中国大地坐标系(China Geodetic Coordinate System 2000，CGCS2000)，又称之为2000国家大地坐标系，是中国新一代大地坐标系，21世纪初已在中国正式实施。

## 常见坐标系之间的相互转换

坐标系转换手段：

1. 在线转换网站：[epsg.io](https://epsg.io/transform)
2. 代码层面：[proj4.js](https://github.com/proj4js/proj4js)
3. 一般转换都是有公式的，不想引用库的话一般都需要自己封装util，将各个公式用js实现，当然网上也很多。

常用坐标系转换：

1. **3857 -> 4326**
2. **4326 -> 3857**
3. **CGCS2000 -> 4326**

对于三维来说，常见的有经纬度、世界坐标系、屏幕坐标系等转换：

以cesium为例：[Cesium中的几种坐标和相互转换](https://github.com/AJJackGIS/Cesium/blob/master/doc/Cesium%E4%B8%AD%E7%9A%84%E5%87%A0%E7%A7%8D%E5%9D%90%E6%A0%87%E5%92%8C%E7%9B%B8%E4%BA%92%E8%BD%AC%E6%8D%A2.md)





## 面试问题汇总

2023.06.08

1. 3857和4326之间坐标系互转公式原理
2. 天地图格式，天地图20级别0 0 坐标在哪里？
3. 百度或者高德地图坐标放到openglayers中不正确是什么原因
4. geojson格式，如何展示矢量数据的，multipolygon 与polygon的区别
5. 有一个镂空的窗户，是用multipolygon展示还是用polygon展示
6. three.js加载需要哪几个核心，有哪几种camera —— three.js 面试题要准备
   1. scene：
   2. camera
      1. camera：摄像机的基础类，不能够被直接实例化
      2. perspectiveCamera 使用透视投影
      3. orthographicCamera 使用正交投影
      4. cubeCamera：创建6个渲染到webGLCubeRenderTarget的摄像机
      5. stereoCamera：立体摄像机
   3. renderer
7. proj4 经纬度反了、
8. potree加载数据几种格式和方法、
9. openlayers添加poi点
10. 3dtiles如何做单体化，是数据做单体化，还是前端做单体化，字段是什么
11. 模型数据比较大，如何优化，cesium/three
12. 3d模型数据格式
13. 点云数据格式
14. openlayers如何加载点云数据的
15. openlayers如何加载矢量数据的
16. 你知道哪些常见的抗锯齿方法以及其原理？cesium中有哪些抗锯齿方法？
    1. [抗锯齿分类](https://zhuanlan.zhihu.com/p/159171397)
    2. MSAA
    3. SSAA
    4. FXAA
    5. MLAA
    6. 。。。
17. cesium中有哪些后处理？并说明其含义？

