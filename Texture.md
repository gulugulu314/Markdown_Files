**UGUI竟然是开源，找到源代码，看一下??


** sprite的 outuv 和  inneruv的区别
Datautiltiy.GetOuterUV,GetInnerUV‘

- GetOuterUV:范围是（0，0）~（1，1），如果图片打包成图集，则为图片在图集中的位置
- GetInnerUV：一般和GetOuterUV相同；
- GetPadding:一般为Vector4.zero
- GetMinSize：一般为Vector4.zero


** Unity打包图集 创建Sprite Atlas,加载图集等；
unity自带的sprite atlas，会选择文件夹，进行打包图集，打包完了还需要对每个sprite进行标注packingtag,
可以用sprite packer查看图集


** 性能优化的策略一般是：CPU的drawcall > overdraw> 面片数




# Texture 纹理

- **NPOT Texture** : Non-power-of-two texture 非二次方纹理


//

### **Unity遮罩**
  
#### 1、遮罩的原理

#### 2、实现遮罩的方式

- **方式一：添加原生Mask遮罩**

    添加原生的Mask，需要在子物体的父物体上，添加遮罩Mask组件，Mask组件依赖Image组件，可以通过Image设置Mask的形状；

    - **优势：**

        方便、简单，可以快速定制各种不同的遮罩图层

    - **劣势：**

        - Unity会额外消耗一个drawcall来创建mask,做像素剔除，增加overdraw的渲染；
        - 影响层级合并，同一个层级的UI可以合并，仅需一个drawcall渲染，加入mask后，两者不能合并，一个ui被分割开来，至少需要两个drawcall，影响层级合并的效率；
        - 无法精确点击，例如圆形的遮罩，边缘部分会点击到；（可以通过设置eventAlphaThreshold属性，根据像素点是否已经超过阈值来判断点击出发，要求传入必须是Spirte,需要勾选Read/WriteEnabel,增加两倍内存，并且无法打包进入图集）

- **方式二：绘制Mesh,修改Image的Mesh,对Image做扩展**

    通过OnPopulateMesh的方式，重新绘制Mesh
    需要解决点击的问题，就是在图像为覆盖的地方，依然能够点击到，是因为Unity默认的触发框是矩形的包围盒


    - **优势：**



    - **劣势：**

- **方式三： 通过Shader实现**

    - **优势：**



    - **劣势：**