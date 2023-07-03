# CG知识点汇总

> 个人遇到的问题或关键知识点，可能会比较零散



###### 1. 向量点乘的意义？

向量的点乘，也叫向量的内积、数量积，对两个向量执行点乘运算，就是对这两个向量对应位一一相乘之后求和的操作，点乘的结果是一个标量。

**几何意义：**

* 可以用来表征或计算两个向量之间的夹角。

* b向量在a向量方向上的投影。

由向量点乘获取的夹角值可进一步判断这两个向量是否是同一方向，是否正交（也就是垂直）等方向关系，具体关系为：

* a . b > 0  方向基本相同，夹角在0度到90度之间
* a . b = 0  正交，相互垂直
* a . b < 0  方向基本相反，夹角在90度到180度之间

---



###### 2. 向量叉乘的意义？

在三维几何中，向量a和向量b的叉乘结果是一个向量，更为熟知的叫法是法向量，该向量垂直于a和b向量构成的平面。

在三维图像学中，叉乘的概念非常有用，可以通过两个向量的叉乘，生成第三个垂直于a、b的法向量，从而构建x、y、z坐标系。

在二维空间中，叉乘还有一个几何意义：a X b等于由向量a和向量b构成的平行四边形面积。

---



###### 3. 什么是图形渲染管线？

opengl中的大部分工作是将3D坐标转换为2D像素，而这部分工作主要就是通过图形渲染管线来实现的。其大致含义就是将原始数据输送进管道，期间经过各种变换处理最终转为像素展示在屏幕是上。

图形渲染管线主要包括两个部分：

* 第一部分，将3D坐标转为2D坐标
* 第二部分，将2D坐标转变为实际的有颜色的像素。

图形渲染管线的几个步骤：

* **顶点着色器：Vertex Shader**

  单独的顶点作为输入。顶点着色器的主要目的是把3D坐标转换为另一种3D坐标，同时允许对顶点属性做进一步的操作。

* **图元装配：Primitve Assembly**

  将顶点着色器输出的所有顶点作为输入，将所有的点装配成指定的图元形状。

* **几何着色器：Geometry Shader**

  图元装配阶段的输出会传递给几何着色器。几何着色器把图元形式的一系列顶点的集合作为输入，它可以通过产生新顶点构造出新的（或者其它的）图元来生成其他形状。

* **光栅化：Rasterization**

  几何着色器的输出作为光栅化的输入，会把图元映射为屏幕上的像素，生成供片段着色器执行的片段（Fragment）；在片段着色器执行之前会执行裁切（clip），丢弃视图范围外的像素。

* **片段着色器：Fragment Shader**

  片段着色器的主要目的是计算一个像素的最终颜色，这里也是openGL高级效果产生的地方。通产片段着色器包含3D场景数据（光照、阴影、光的颜色等），这些数据可以被用来计算像素的最终颜色。

* **Alpha测试和混合（Blending）：Tests And Blengding**

  这个阶段检测片段对应的深度值（z-buffer），用它们判断这个像素是其他物体的前面还是后面，决定是否应该丢弃。这个阶段也会检查alpha值并对物体进行混合（blend）。所以，即使在片段着色器中计算出来了一个像素的颜色，在渲染多个三角形的时候最后的像素颜色也可能完全不同。

  

即：顶点着色器-> 形状(图元)装配->几何着色器->光栅化->片段着色器->测试与混合

---



###### 4. 什么是VBO、VAO、EBO以及与glEnableVertexAttribArray和glVertexAttribPointer的关系？

1. **VBO：**

   顶点缓冲对象，Vertex Buffer Object；管理GPU上一块内存（显存），其用于储存顶点信息、颜色信息、法线信息、纹理坐标信息以及索引信息等等。

   VBO是CPU和GPU之间传递信息的桥梁，我们把数据存入VBO（这一步在CPU执行），然后VBO会自动把数据送入GPU。送入GPU这一步，不需要任何人为操作，用户只负责往VBO中存入数据就可以了。

   但是对于GPU来说，VBO中存的就是一堆数字而已，要怎么解释它们呢？这就要用到VAO了

2. **VAO：**

   顶点数组对象，Vertex Array Object；一个VAO可以有多个VBO，它是一个容器，它将所有可以由glVertexAttribPointer和其它一些函数进行设置的状态包装到一起。

   VBO是为了向GPU传递顶点数据，那么VAO就是为了向GPU解释顶点数据。如:

   ```c++
   float buffer = {
      	//顶点坐标(3个一组)              //顶点颜色（3个一组）              //纹理坐标(2个一组)
       0.5f,  0.5f, 0.0f,                1.0f, 0.0f, 0.0f,               1.0f, 1.0f,
       0.5f, -0.5f, 0.0f,                0.0f, 1.0f, 0.0f,               1.0f, 0.0f,
      -0.5f, -0.5f, 0.0f,                0.0f, 0.0f, 1.0f,               0.0f, 0.0f,
      -0.5f,  0.5f, 0.0f,                1.0f, 1.0f, 0.0f,               0.0f, 1.0f
   };
   //以上数据仅为本博文编造，不具有实操意义		
   ```

   由上可见，顶点数据并非只是三个为一组的三维坐标！若向VBO中传入如上述buffer数据，并且VBO把它们传入了GPU；下一步由顶点着色器进行读取，然而顶点着色器并不知道该如何解释这些数字，到底是把它们3个一组，还是3个一组、后2个一组、或者是别的组合... 即GPU并不知道。

   这就需要VAO的参与了，它负责告诉GPU，VBO中的信息到底该以几个为一组、起始数据、步长等等。

   ```c++
   	//vertex coord
   glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)0);
   glEnableVertexAttribArray(0);
   
   // color attribute
   glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(3 * sizeof(float)));
   glEnableVertexAttribArray(1);
   
   // texture coord attribute
   glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(6 * sizeof(float)));
   glEnableVertexAttribArray(2);
   ```

3. **EBO：**Element Buffer Object——元素缓冲对象或**IBO**——索引缓冲对象Index Buffer Object。EBO是一个缓冲区，就像顶点缓冲区对象一样，它存储OpenGL用来决定要绘制哪些顶点的索引，这种绘制方式，可以减少顶点叠加造成的不必要开销。

---



###### 5. 什么是Texture Filtering （纹理插值方式、纹理过滤、纹理滤波 ）？

1. 为什么在纹理采样时需要Texture Filtering？

   百度百科：在计算机图形学中，纹理滤波是一种针对一个使用材质贴图的像素，使用临近的一个或多个纹素计算其纹理颜色的方法。从数学上来说，纹理滤波是抗锯齿的一种，但它更着重于滤掉纹理中的高频，而不像其他抗锯齿技术那样着重用于改善边界显示效果。简单来说，它使得同一个纹理可以被用于不同的形状，尺寸和角度，同时尽可能减少显示时的模糊和闪烁。用户可以权衡计算复杂度和图像质量，在许多种纹理滤波方法中进行选择。

   我们的纹理是要贴到三维图形表面的，而三维图形上的pixel中心和纹理上的texel中心并不一致（pixel不一定对应texture上采样的texel），大小也不一定一致。

   当纹理大于三维图形表面时，导致一个像素上被映射了多个纹素，从而可能致使贴图变得错位；当纹理小于三维图形表面时，导致多个像素上映射了同一个纹理，从而可能致使贴图变得模糊。

   要解决此类问题，必须通过技术平滑texel和pixel之间的对应关系，这种技术就是Texture Filtering；而平滑两者对应关系所采取的不同方式就是纹理过滤选项，在一般的图形API中都会枚举出来。

2. 纹理过滤选项

   根据视角与模型表面是否有夹角，针对采样点是正方形或正方体分布和非正方形或正方体分布，可以将过滤选项分为两类：各向同性滤波与各向异性滤波

   1. 各项同性滤波：正方形mipmap
      1. GL_NEAREST：也叫邻近过滤，Nearest Neighbor Filtering；是opengl默认的纹理过滤方式。当设置为该选项时，openGL会选择中心点最接近纹理坐标的那个像素。
      2. GL_LINEAR：也叫线性过滤，Linear Filtering；它会基于纹理坐标附近的纹理像素，计算出一个插值，近似出这些纹理像素之间的颜色。一个纹理像素的中心距离纹理坐标越近，那么这个纹理像素的颜色对最终的样本颜色的贡献就越大。针对于纹理坐标“附近”的概念（即距离纹理坐标多远的像素可以被采样），结合mipmap技术又可衍生出多种过滤方式；常见的有双线性滤波和三线性滤波。
      3. 最近邻Mipmapping
      4. 双线性滤波：在采样点最近的四个纹素上加权平均。但它只作用于一个Mipmap Level，它选取texel和pixel之间大小最接近的那一层Mipmap进行采样。当和pixel大小匹配的texel大小在两层Mipmap level之间时，双线性过滤在有些情况下效果不是很理想，于是就有了三线性过滤。
      5. 三线性滤波：三线性滤波以双线性滤波为基础，会对pixel大小与texel大小最接近的两层mipmap level分别进行双线性过滤，然后再对两层得到的结果进行加权平均。三线性过滤在一般情况下已经非常理想了。但是到目前为止，我们均假设texture投射到屏幕空间是各向同性的。但对各向异性来说效果就不理想了，于是产生了各向异性过滤。
   2. 各向异性滤波（Anisotropic Filtering）：[各向异性过滤](https://baike.baidu.com/item/各向异性过滤/662077?fromModule=lemma_inlink)是现有消费级显卡所提供的图像质量最佳的滤波方式。传统的各项同性滤波中只是正方形的mipmap层次间进行双线性或三线性插值，但是当一个目标表面和摄像机之间的角度较大时，纹理的填充面积并不是正方形，这样便引起了模糊和闪烁等瑕疵。于是，各向异性滤波需要对一个非方形纹理进行采样。在一些简单的实现中，显卡使用长方形的纹理取代方形纹理，达到了较好的近似效果。但是这种方式在处理边界时效果依然不理想，原因是在倾斜的表面上，近端边界比远端边界拥有更多的像素。于是一些高端显卡使用了梯形纹理，当然这也要求更大的运算量。

---



###### 6. 什么是Mipmaps（多级渐远纹理）？

物体较远时，所占的像素片段很少，此时从纹理中为这些片段获取正确的颜色值就很困难，因为它需要对一个跨过纹理很大部分的片段只拾取一个纹理颜色。在小物体上会产生不真实的感觉，并且若再采用高分辨率纹理会浪费内存。

为了解决此类问题，引入了Mipmap，它简单的来说就是一系列的纹理图像，后一个纹理图像是前一个的二分之一。多级渐远纹理背后的理念很简单：距观察者的距离超过一定的阈值，openGL会使用不同的多级渐远纹理（图像），即最**适合**物体的距离的那个。

---



###### Shading01

- Blinn-Phong reflectance model
- Shading models / frequencies

---



###### Shading02

* Graphics Pipeline
* Texture mapping

---



###### Shading03

* Barycentric corrdinates
* Texture antialiasing (MipMap)  纹理太小 --> 插值，纹理太大--> 范围查询
* Applications of Textures [[计算机图形学|自学记录]纹理的高级应用](https://zhuanlan.zhihu.com/p/628729927)

凹凸贴图 == 法线贴图



###### Geometry相关

计算机图形学中对几何进行归类

1. Implicit

   * algebraic sruface
   * level sets
   * distance functions
   * ...

   

2. Explicit

   * point cloud
   * polygon mesh
   * subdivision , NURBS
   * 。。。


---



###### 7. 介绍一下什么是冯氏光照模型(Phong Lighting Model)？

```glsl
// 冯氏光照模型
vec3 result = (ambient + diffuse + specular) * objectColor;
FragColor = vec4(result, 1.0);
```

1. 环境光照(Ambient Lighting)：即使在黑暗的情况下，世界上通常也仍然有一些光亮（月亮、远处的光），所以物体几乎永远不会是完全黑暗的。为了模拟这个，会使用一个环境光照常量，它永远会给物体一些颜色。**说白了场景中默认是有环境光照的，一般用一个环境光照系数表示。**

   ```glsl
      void main()
      {
          float ambientStrength = 0.1;
          vec3 ambient = ambientStrength * lightColor;
      
          vec3 result = ambient * objectColor;
          FragColor = vec4(result, 1.0);
      }
   ```


2. 漫反射光照(Diffuse Lighting)：模拟光源对物体的方向性影响(Directional Impact)。它是冯氏光照模型中视觉上最显著的分量。物体的某一部分越是对着光源，它就会越亮。说白了**漫反射是跟角度有关系**。

   详细计算请参考[LearnOpenGL-基础光照](https://learnopengl-cn.github.io/02%20Lighting/02%20Basic%20Lighting/)

3. 镜面光照(Specular Lighting)：模拟有光泽物体上面出现的亮点。镜面光照的颜色相比于物体的颜色会更倾向于光的颜色。**说白了，物体表面越光滑，其光照颜色更倾向于光。**

   详细计算请参考[LearnOpenGL-基础光照](https://learnopengl-cn.github.io/02%20Lighting/02%20Basic%20Lighting/)















