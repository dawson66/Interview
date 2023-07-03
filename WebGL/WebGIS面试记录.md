

##### 苏州园区测绘面试记录 —— 2023.06.08

###### 1. 3857和4326之间坐标系互转公式原理

###### 2. 天地图格式，天地图20级别0 0 坐标在哪里？

###### 3. 百度或者高德地图坐标放到openglayers中不正确是什么原因

###### 4. geojson格式，如何展示矢量数据的，multipolygon 与polygon的区别

1. Polygon定义：A Polygon is a planar Surface defined by 1 exterior boundary and 0 or more interior boundaries. Each interior boundary defines a hole in the Polygon.即，Polygon多边形是由 1 个外部边界和 0 个或多个内部边界定义的平面。每个内部边界在多边形中定义一个洞。**明确Polygon是一个线性环的概念**

###### 5. 有一个镂空的窗户，是用multipolygon展示还是用polygon展示

1. 用polygon展示[Gis的polygon和multipolygon](https://blog.csdn.net/wzwxwc1987/article/details/88582993)

###### 6. three.js加载需要哪几个核心？有哪几种camera？

* scene：

* camera
  * camera：摄像机的基础类，不能够被直接实例化
  * perspectiveCamera 使用透视投影
  * orthographicCamera 使用正交投影
  * cubeCamera：创建6个渲染到webGLCubeRenderTarget的摄像机
  * stereoCamera：立体摄像机
* renderer

---

###### 7. proj4 经纬度反了是什么原因？

Order Axis：默认情况下，proj4对投影（笛卡尔）坐标系使用`[x, y]`轴顺序，对地理坐标系使用`[x=longitude, y=latitude]`。对x和y相反排序可以设置`forward`或`inverse`方法的第二个参数`enforceAxis`来设置；enforceAxis是`Boolean`类型，当设置为true时，x和y将对调位置。		

```typescript
proj4(fromProjection, toProjection).forward(coordinate, enforceAxis)
proj4(fromProjection, toProjection).inverse(coordinate, enforceAxis)
```

---

###### 8. potree加载数据几种格式和方法？

方法：`Potree.loadPointCloud(path, name, callback)`

path有三种：

* `ept.json`
* `cloud.js`
* `metadata.json`

---

###### 9. openlayers添加poi点

添加point feature ，设置其style为一些图标 图像等。

---

###### 10. 3dtiles如何做单体化，是数据做单体化，还是前端做单体化，字段是什么？

**单体化**：听起来高大上，实际上就是将一整个模型进行划分，目的是让局部能够拾取；例如一座大桥，鼠标想拾取桥面、栏杆、顶部支撑杆等等。

将模型依据一定的规则或者属性进行划分，就是单体化的过程。正常就两种做法：

1. **通过模型数据内部属性进行单体化。**

   如：[官方案例](https://sandcastle.cesium.com/?src=3D%20Tiles%20Feature%20Styling.html )

   `cesium3DTileset`字段`classificationType`可以确定`terrian`、`3D Tiles`	是否分类。

2. **矢量面叠加单体化**：需要构造单体化矢量数据

   a：正常添加3dtiles模型

   b：绘制矢量面。核心在这一步，其通过一些软件来绘制矢量面（shp文件）。

   c：将矢量面叠加到3dtiles模型上。

   d：添加一个移动的面及样式，绑定移动事件；移动时获取当前选中的面的坐标，设定移动面的坐标为当前选中的面。

   ---

###### 11. 模型数据比较大，如何优化，cesium/three？

###### 12. 3d模型数据格式有哪些？

* OSGB
* Obj
* Fbx
* Las/Laz
* 3ds

###### 13. 点云数据格式有哪些？

* pts
* Las
* Laz
* pcd
* .xyz

---

###### 14. Las和Laz点云文件的区别？

* Las与Laz相比，其占用磁盘内存减少，大概四倍左右。
* Laz较Las读取时间降低。

###### 15. openlayers如何加载点云数据的

旧版本的openlayers 有`WebGLPointLayer`图层，新版本取消了该图层；可使用`renderer/webgl/PointsLayer`优化

###### 16. openlayers如何加载矢量数据的

###### 17. 你知道哪些常见的抗锯齿方法以及其原理？cesium中有哪些抗锯齿方法？

参考：[抗锯齿分类](https://zhuanlan.zhihu.com/p/159171397)

1. MSAA
2. SSAA
3. FXAA
4. MLAA
5. 。。。

###### 18. cesium中有哪些后处理？并说明其含义？

cesium中的后处理主要集中在`postProcessStageCollection`和`postProcessStageLibrary`两个类中，其中抗锯齿fxaa、环境光遮蔽ambientOcclusion和泛光bloom作为`postProcessStageCollection`类的成员属性存在，其余的后处理在`postProcessStageLibrary`类中以成员方法存在，以`postProcessStageLibrary.create后处理名称` API形式存在。

1. 抗锯齿：fxaa
2. 环境光遮蔽：ambientOcclusion
3. 泛光：bloom
4. 黑白渐变：blackAndWhiteStage
5. 高斯模糊：gaussian blur 一种滤镜功能
6. 全局亮度：brightness
7. 景深效果：depthOfFiled
8. 边缘检测：edgeDetection，边缘检测是图像处理和计算机视觉中的基本问题，边缘检测的目的是标识数字图像中亮度变化明显的点。图像属性中的显著变化通常反映了属性的重要事件和变化
9. 轮廓效果：silhouette
10. 夜视效果：night vision

###### 19. three.js中drawCall的理解？作用？

###### 20. openlayers 吸附snap原理？

###### 21. openlayers 实现undo redo原理？

###### 22. 说一说mapbox style expression （样式表达式）？规则是什么？有什么作用？

###### 23. 什么是PBR？

