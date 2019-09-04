
米图灵动模型处理编辑器说明文档

---

- **说明**

  - Unity2019版本以上，所有的预设体操作都应该Unpack Prefab以后进行操作；
  

- ***建筑模型处理流程图一***

![处理流程图](E:/7_GitHub/Markdown_Files/Image/处理流程图.png)  

- ***建筑模型处理流程图二***

![处理流程图2](E:/7_GitHub/Markdown_Files/Image/处理流程图2.png)  

# 模型编辑器-建筑

建筑模型处理包含两个部分：**外观建筑模型、内建筑模型**；

**其中：**

    外观建筑模型是由3DMax制作，导出的.Fbx文件，文件中内嵌纹理贴图及材质；

    内建筑模型是由Revit软件制作，导出.Fbx文件，可以是裸模，不需要材质信息；


- **外建筑提供的资源：**
   
  文件资源命名：**院区名_日期.FBX**,例如：**SZKNHP-20190601.FBX文件**
  
    ![外建筑模型提交资源格式](E:/7_GitHub/Markdown_Files/Image/OutdoorMaxModel.png)              
- **外建筑层级结构：**
  
  外建筑分为三个部分：**buildings,ground,others**

  其中：buildings是划分好楼层的建筑，ground是地面，后期会加入Tree,others主要是一些装饰物体

    ![外建筑层级](E:/7_GitHub/Markdown_Files/Image/outdoorLevel.png)              

    **buildings的层级：**

    - buildings节点下为各建筑，**每个建筑的中心点应该位于建筑的中心**，在3dMax建模时应该调整正确；
    - 建筑节点（1）下为各楼层（2），每一层对应一个box（4），每一层下有自己的outdoor节点，outdoor节点下的mesh为该层的实体外建筑（3）；
    - 楼层节点（2）与box节点（4）为**同一层级**；
    - 楼层节点的中心点应该是该楼层的中心点；
    - **楼层Box的编码**：楼层编码_Box,例如：**BDHCMU-A02-F002_Box**;
    - 每个建筑下都应该有一个建筑Box(5)，建筑Box的编码为:院区-建筑_Box，例如：

    ![外建筑层级](E:/7_GitHub/Markdown_Files/Image/buildingLevel.png)    

- **内建筑提供的资源：**

    - **Revit文件**，Revit文件的提交格式：**院区-建筑-AR.rvt**,例如：**TSCENH-K03-AR.rvt**
  
        ![外建筑层级](E:/7_GitHub/Markdown_Files/Image/indoorModel.png)   

    - **Excel表**，该Excel表统计每个房间的部门、用途等信息属性;
    
        *注：若使用翻模插件翻模，可直接生成Excel文件*

        ![Excel](E:/7_GitHub/Markdown_Files/Image/Excel.png)   


- **内建筑检查**

    - 信息检查
        - 限制条件中的各属性信息标注是否完整
    - 楼板检查
        - 检查是否出现不应该适用楼板绘制的模型，确保所有的楼板都能够编码
    - 楼层检查
        - 楼层编码是否符合建模规范
    - 内外墙和内外柱检查
        - 内外柱和内外墙应该区分开，在模型的族名称中标注出，
        - 翻模软件翻模的建筑，应该人工标注


## 外建筑划分

![外建筑划分](E:/7_GitHub/Markdown_Files/Image/外建筑划分.png)   

主要分四个部分：

- **调整外建筑材质**
  
    - 外建筑使用标准的Unity StandardShader，材质中使用贴图的灰度值，Unity默认降低一倍，使用Editor将灰度值调整到最亮；

    - **模型根节点应该选择Fbx根节点**，调整整个建筑的材质球，在调整之前应该确保FBX文件中的材质已经解析出来；

    ![调整外建筑材质](E:/7_GitHub/Markdown_Files/Image/调整外建筑材质.png)   

    ![解析外建筑材质](E:/7_GitHub/Markdown_Files/Image/解析外建筑材质.png)   

- **3dMax划分**

    - **模型根节点选择buildings**

    - **3dMax划分后的层级结构**

        外建筑划分后的层级结构为：**others,boxes，levels**

        boxes：除了包含每层的box后，还包含一个BuildingBox,BuildingBox下又复制了一份楼层的box；

    ![外建筑划分后](E:/7_GitHub/Markdown_Files/Image/外建筑划分后.png)   

- **设置BoxLayer**

    - **模型根节点选择buildings**

    - 设置Box的layer为：buildingBox,并自动添加MeshCollider

    ![BoxLayer](E:/7_GitHub/Markdown_Files/Image/BoxLayer.png)   

- **设置BoxMaterial**

    - **模型根节点选择buildings**

    - Box的材质球为：不需要选择

    ![BoxMaterial](E:/7_GitHub/Markdown_Files/Image/BoxMaterial.png)   



## 内建筑层级

    内建筑导入到Unity之前，需要使用Pixel软件或者Simplygon软件进行优化、优化后的模型需要导入到3dMax内
    调整轴心位置，调整完位置后，导入到Unity做下一步处理


- 流程

```flow
st=>start: revit文件
e=>end
op1=>operation: 模型检查
op2=>operation: Simplygon/PiXYZ优化
op3=>operation: 3dMax轴心点处理
op4=>operation: 导入Unity调整层级
st->op1->op2->op3->op4
```


- **Pixel软件优化**

使用Pixyz软件进行优化，一般设置Preset为Strong即可

![Pixyz](E:/7_GitHub/Markdown_Files/Image/Pixyz.png)   

- **Simplygon优化**


- **3dMax调整轴心**

3dMax的单位设置，应该设置为厘米（Centimeter）,设置好单位后，调整轴心导出即可

![3dMax操作2](E:/7_GitHub/Markdown_Files/Image/3dMax操作2.png)   ![3dMax操作](E:/7_GitHub/Markdown_Files/Image/3dMax操作.png)   

- **内建筑层级调整**

    - PiXYZ软件导出的模型结构如下图：

    ![revit文件](E:/7_GitHub/Markdown_Files/Image/revit文件.png)   

    - 调整层级

    - **模型根节点选择Revit模型的根节点**
    
    ![内建筑层级](E:/7_GitHub/Markdown_Files/Image/内建筑层级.png)   

## 内建筑划分

 ![内建筑划分](E:/7_GitHub/Markdown_Files/Image/内建筑划分.png)   

 - 划分后的层级为 ：
   
![内建筑划分后](E:/7_GitHub/Markdown_Files/Image/内建筑划分后.png)   


**选择：**

- 模型根节点
- 数据库文件

**操作：**

- **建筑类型划分（整体）** 
  
    - 根据数据库中的表，将模型按不同的类型进行划分开，
    - 一般分为：**墙（Wall）**、**楼板(Floor)**、**结构柱（StructureColumn）、窗(Window)**、**门(door)、幕墙嵌板(CurtainWallJamb)**、**幕墙竖梃(CurtainWallPanel)，栏杆扶手(Railing)**，**楼梯(stairs)**、**天花板(celling)、屋顶(roof)** 等，确保数据库中包含所有模型的表；
    - 有些为分配的，但场景中有重复的，例如楼梯的**整体平台和整体梯段可以删除**；
    - 建筑划分完成后，需要单独选择一块每一层的楼板作为划分**楼层标高的标识**；创建一个空物体，将所有的楼板放置到空物体下
    - 获得所有楼板标识后，可以还原整体建筑划分；

- **建筑楼层划分**

    - 划分楼层，需要根据楼板标识进行划分，选择划分楼板的节点；
    - 指定当前划分建筑的编码
    - 指定当前建筑地下层数，如果中间有夹层部分，需要手动修改楼层编码
    - 如果是多建筑的话，需要根据数据库进行划分

- **建筑类型划分（楼层）** 

    - 划分好楼层后，既可划分楼层内的类型；

**注意：** <font color = "#ff0000">在编码之前应该保存一份位编码的内建筑，以便后期有修改；
未编码内建筑可以标注为UnEncode,例如TCENH-K03-AR-UnEncode</font>



## 内建筑命名

**选择：**

- 模型根节点
- 数据库文件

![内建筑命名](E:/7_GitHub/Markdown_Files/Image/内建筑命名.png)  

**操作：**

- **楼板编码检测：** 
  
    - **测试楼板编码中是否有重复编码**

    - 模型根节点选择：处理的模型根节点
    - 处理完后一定检查是否有重复编码
  
- **楼板命名：** 

    - 给楼板进行编码，根据数据库db文件中<font color = "#ff0000">楼板</font>表，楼板表中的<font color = "#ff0000">空间编码</font>字段必须不为空
    - 仅处理Floor节点下的对象，如果数据库中的楼板表中空间编码字段为空，所有的编码都将变成以<font color = "#ff0000">NE</font>开头的编码
    - 空间编码格式为：<font color = "#ff0000">TSCENH-K03-F002A-RM001</font>
    - 空间编码中楼层后边一定带<font color = "#ff0000">A</font>
    - **处理后的编码**
  
        ![楼板编码](E:/7_GitHub/Markdown_Files/Image/楼板编码.png)  

- **门命名：**

   - 根据需求做处理，一般可以不做处理
   - 如果需要处理，仅对Door节点下的模型处理
   - Door命名编码为：楼层-ID, <font color = "#ff0000">TSCENH-K03-F002-2931120</font>
   - **处理后的编码**
  
        ![门编码](E:/7_GitHub/Markdown_Files/Image/门编码.png)  

- **病床命名：**

- **墙和柱命名**

  - 根据需求做处理，一般可以不做处理
  - 如果建筑模型按模板来做，墙和结构柱的内外之分，在模板里面已经设置好了，不需要做处理
  - 如果不是按模板做，需要根据数据库中的参数来设置
    - 所在表名：你需要处理的表名，例如墙，在数据库中为墙，结构柱在数据库中为结构柱
    - 所在表列：即区分内外的参数所在表列

- **修改楼板编码**
  如果楼板编码中缺少编码A,可以使用此功能快速添加A

## 内建筑替换材质

 ![内建筑材质](E:/7_GitHub/Markdown_Files/Image/内建筑材质.png)  

**选择：**

- 模型根节点

**操作：**

建筑材质的默认路径为：“.../Base/Shared/Materials/BuildingMaterial”,可以不用选择

![建筑材质](E:/7_GitHub/Markdown_Files/Image/建筑材质.png)  

默认全部替换，若只修改某个节点下的材质，可以使用**杂类->替换材质**


## 内建筑设置Layer/Tag

![内建筑LayerTag](E:/7_GitHub/Markdown_Files/Image/内建筑LayerTag.png)  

**选择：**

- 模型根节点

**操作：**

FloorTag默认为：Building_Floors
Layer默认为：buildingLevels

分别点击设置楼板Tag,设置内建筑Layer并关闭阴影


**注意：** <font color = "#ff0000">内建筑处理完成后，可以标注为Encode,例如TCENH-K03-AR-Encode</font>



## 内建筑导入外建筑打包

![内建筑导入Max层级](E:/7_GitHub/Markdown_Files/Image/内建筑导入Max层级.png)  

**选择：**

- 模型根节点

**操作：**

- 内建筑的的层级结构应该调整为正确的编码：

    ![正确层级](E:/7_GitHub/Markdown_Files/Image/正确层级.png)  

- 内建筑的Level层级应与外建筑一致
  

## 墙与楼板的映射关系

用于建筑装修，需要知道楼板和墙面对应关系

![墙和楼板的对应关系](E:/7_GitHub/Markdown_Files/Image/墙和楼板的对应关系.png)  

**选择：**

- 模型根节点
- 选择数据库

**操作：**




## 楼板新旧模型的映射关系

楼板因为编码的缘故造成以前的编码出现问题，可以使用射线映射的方式，获得新旧楼板的对应关系

![楼板新旧模型的映射关系](E:/7_GitHub/Markdown_Files/Image/楼板新旧模型的映射关系.png)  

**选择：**

- 模型根节点
- 选择数据库

**操作：**

- 选择旧模型的根节点作为模型根节点

- 新旧模型上下位置错开一定距离，用于射线检测

- 创建楼板映射关系后，找到对应关系的楼板会变红

- 数据库有两个字段：oldFloorCode,newFloorCode
  
- 保存数据库即可

```

```

# 模型编辑器-管道

## 管道替换

## 管道划分

## 管道材质

## 管道Layer

## 标记竖管

## 管道编码

## 更新数据库

## 管道终端与房间的映射关系



# 模型编辑器-设备

## 设备与房间的映射关系

## 设备Layer/Collider

## 设备细分

## 设备特殊情况
















- 使用单个分号*或者下滑线_表示斜体，或者使用快捷键ctrl+I；
  (使用快捷键的前提，是必须安装Markdown All in One插件)

    - *倾斜字体1*
    - _倾斜字体2_

## 粗体
- 使用两个分号**或者两个下划线__表示粗体，或者使用快捷键Ctrl+B；
    - **加粗字体**
    - __加粗字体__
  
## 高亮
- 使用两个等于号==对字体进行高亮显示
    - ==高亮字体==

# 列表
## 无序列表
    无序列表用分号"*"和中划线"-"，表示一个无序列表
    嵌套列表，继续使用；
    - 列表1
    - 列表2
      - 子列表2.1
      - 子列表2.2
    - 列表3
      - 子列表3.1
      - 子列表3.2
      - 子列表3.3

## 有序列表
    有序列表用数字和.，表示的一个有序列表
    1. 列表1
    2. 列表2
        2.1 子列表2.1
        2.2 子列表2.2
    3. 列表3

# 链接和图片
## 链接
    [链接描述](链接地址) [apple](http://www.apple.com)

## 图片
    ![照片](照片地址)

```
 [苹果网站地址](http://www.apple.com)
 ![单个照片](https://github.com/gulugulu314/Markdown_Files/tree/master/Image/testImage.jpg)
```

 [苹果网站地址](http://www.apple.com)
 ![单个照片](http://meetoo.cn:1986/Patch/Image/Image.jpg)

# 引用
这是一个名人名句的引用：
>《望岳》
>作者:[杜甫](https://baike.baidu.com/item/%E6%9D%9C%E7%94%AB/63508?fr=aladdin)
>岱宗夫如何？齐鲁青未了。
>造化钟神秀，阴阳割昏晓。
>荡胸生曾云，决眦入归鸟。
>会当凌绝顶，一览众山小。
>> 这个是第二级嵌套应用；

# 分割符
使用多个字符，例如---，***，___表示字符；

---

# 代码行或代码块
使用两个\`可以用来显示单行代码或者，进行特殊标记，例如：
- 对某个`字符`特殊标记
- 代码行： `int i = 10;`
- 代码块：使用```表示代码块，为了凸显语言高亮，可以在其后边添加特定语言，例如java,c#,typescript等，可以加上不同的.class{.class1}，{.class2}等。例如：需要显示行号，需要在之后加上 {.line-numbers}


```TypeScript {.line-numbers}
public static Move(target:Sprite3D,position:Vector3,duration:number,ease?:Function,complete?:Handler,delay?:number,coverBefore?:boolean,autoRecover?:boolean):Laya.Tween{
    if(target == null) return null;
    let pos = target.transform.position;
    let tween = Laya.Tween.to(pos,{
            x:position.x,
            y:position.y,
            z:position.z,
            update:new Handler(target,function(){
                this.transform.position = pos;
            })
        },duration,ease,complete,delay,coverBefore,autoRecover);
    return tween;
}
```

# 任务列表
任务列表可以用- [ ] 来表示，如果已完成用- [x]表示

```
- [ ] 未完成项目
- [x] 已完成项目
```

- [ ] 未完成项目
- [x] 已完成项目

# 表格
写入表格需要表头：  
可以控制表格内的格式，用
- :------ 表示靠左对齐；
- ------: 表示靠右对齐；
- :-----：表示居中对齐；

``` 
| 一个普通标题 | 一个普通标题 | 一个普通标题 |
| :------ | ------ | ------: |
| 短文本 | 中等文本 | 稍微长一点的文本 |
| 稍微长一点的文本 | 短文本 | 中等文本 |
```
| 一个普通标题 | 一个普通标题 | 一个普通标题 |
| :------ | ------ | ------: |
| 短文本 | 中等文本 | 稍微长一点的文本 |
| 稍微长一点的文本 | 短文本 | 中等文本 |

id|name|sex|age
------|:-------|-------|------:|
1|张三|男|18
2|李四|男|22
3|王五|女|19

# TOC
TOC可以快速生成目录


4>5













