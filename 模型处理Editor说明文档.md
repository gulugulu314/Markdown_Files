
米图灵动模型处理编辑器说明文档

---

- **说明**

  - Unity2019版本以上，所有的预设体操作都应该Unpack Prefab以后进行操作；
  

- ***建筑模型处理流程图一***

![处理流程图](http://meetoo.cn:1986/Patch/Image/处理流程图.png)  

- ***建筑模型处理流程图二***

![处理流程图2](http://meetoo.cn:1986/Patch/Image/处理流程图2.png)  

# 模型编辑器-建筑

建筑模型处理包含两个部分：**外观建筑模型、内建筑模型**；

**其中：**

    外观建筑模型是由3DMax制作，导出的.Fbx文件，文件中内嵌纹理贴图及材质；

    内建筑模型是由Revit软件制作，导出.Fbx文件，可以是裸模，不需要材质信息；


- **外建筑提供的资源：**
   
  文件资源命名：**院区名_日期.FBX**,例如：**SZKNHP-20190601.FBX文件**
  
    ![外建筑模型提交资源格式](http://meetoo.cn:1986/Patch/Image/OutdoorMaxModel.png)              
- **外建筑层级结构：**
  
  外建筑分为三个部分：**buildings,ground,others**

  其中：buildings是划分好楼层的建筑，ground是地面，后期会加入Tree,others主要是一些装饰物体

    ![外建筑层级](http://meetoo.cn:1986/Patch/Image/outdoorLevel.png)              

    **buildings的层级：**

    - buildings节点下为各建筑，**每个建筑的中心点应该位于建筑的中心**，在3dMax建模时应该调整正确；
    - 建筑节点（1）下为各楼层（2），每一层对应一个box（4），每一层下有自己的outdoor节点，outdoor节点下的mesh为该层的实体外建筑（3）；
    - 楼层节点（2）与box节点（4）为**同一层级**；
    - 楼层节点的中心点应该是该楼层的中心点；
    - **楼层Box的编码**：楼层编码_Box,例如：**BDHCMU-A02-F002_Box**;
    - 每个建筑下都应该有一个建筑Box(5)，建筑Box的编码为:院区-建筑_Box，例如：

    ![外建筑层级](http://meetoo.cn:1986/Patch/Image/buildingLevel.png)    

- **内建筑提供的资源：**

    - **Revit文件**，Revit文件的提交格式：**院区-建筑-AR.rvt**,例如：**TSCENH-K03-AR.rvt**
  
        ![外建筑层级](http://meetoo.cn:1986/Patch/Image/indoorModel.png)   

    - **Excel表**，该Excel表统计每个房间的部门、用途等信息属性;
    
        *注：若使用翻模插件翻模，可直接生成Excel文件*

        ![Excel](http://meetoo.cn:1986/Patch/Image/Excel.png)   


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
    - 重复楼板绘制
        - 一块楼板下方有重复的楼板
    - 楼板序号错误检测
        - 由于Revit楼层平面视图中，楼层编码不规范，造成序号编码错误的检查（针对在Revit中编号情况）

## 外建筑划分

![外建筑划分](http://meetoo.cn:1986/Patch/Image/外建筑划分.png)   

主要分四个部分：

- **调整外建筑材质**
  
    - 外建筑使用标准的Unity StandardShader，材质中使用贴图的灰度值，Unity默认降低一倍，使用Editor将灰度值调整到最亮；

    - **模型根节点应该选择Fbx根节点**，调整整个建筑的材质球，在调整之前应该确保FBX文件中的材质已经解析出来；

    ![调整外建筑材质](http://meetoo.cn:1986/Patch/Image/调整外建筑材质.png)   

    ![解析外建筑材质](http://meetoo.cn:1986/Patch/Image/解析外建筑材质.png)   

- **3dMax划分**

    - **模型根节点选择buildings**

    - **3dMax划分后的层级结构**

        外建筑划分后的层级结构为：**others,boxes，levels**

        boxes：除了包含每层的box后，还包含一个BuildingBox,BuildingBox下又复制了一份楼层的box；

    ![外建筑划分后](http://meetoo.cn:1986/Patch/Image/外建筑划分后.png)   

- **设置BoxLayer**

    - **模型根节点选择buildings**

    - 设置Box的layer为：buildingBox,并自动添加MeshCollider

    ![BoxLayer](http://meetoo.cn:1986/Patch/Image/BoxLayer.png)   

- **设置BoxMaterial**

    - **模型根节点选择buildings**

    - Box的材质球为：不需要选择

    ![BoxMaterial](http://meetoo.cn:1986/Patch/Image/BoxMaterial.png)   



## 内建筑层级

    内建筑导入到Unity之前，需要使用Pixel软件或者Simplygon软件进行优化、优化后的模型需要导入到3dMax内
    调整轴心位置，调整完位置后，导入到Unity做下一步处理


- 流程

```flow
st=>start: revit文件
e=>end
op1=>operation: 模型检查
op2=>operation: Simplygon/PiXYZ优化，展UV
op3=>operation: 3dMax轴心点处理
op4=>operation: 导入Unity调整层级
st->op1->op2->op3->op4
```


- **Pixel软件优化**

使用Pixyz软件进行优化，一般设置Preset为Strong即可

![Pixyz](http://meetoo.cn:1986/Patch/Image/Pixyz.png)   

优化后，需要对模型展UV，进行后期的楼层烘焙；（需要加入图片）

- **Simplygon优化**


- **3dMax调整轴心**

3dMax的单位设置，应该设置为厘米（Centimeter）,设置好单位后，调整轴心导出即可

![3dMax操作2](http://meetoo.cn:1986/Patch/Image/3dMax操作2.png)   ![3dMax操作](http://meetoo.cn:1986/Patch/Image/3dMax操作.png)   

- **内建筑层级调整**

    - PiXYZ软件导出的模型结构如下图：

    ![revit文件](http://meetoo.cn:1986/Patch/Image/revit文件.png)   

    - 调整层级

    - **模型根节点选择Revit模型的根节点**
    
    ![内建筑层级](http://meetoo.cn:1986/Patch/Image/内建筑层级.png)   

## 内建筑划分

 ![内建筑划分](http://meetoo.cn:1986/Patch/Image/内建筑划分.png)   

 - 划分后的层级为 ：
   
![内建筑划分后](http://meetoo.cn:1986/Patch/Image/内建筑划分后.png)   


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

![内建筑命名](http://meetoo.cn:1986/Patch/Image/内建筑命名.png)  

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
  
        ![楼板编码](http://meetoo.cn:1986/Patch/Image/楼板编码.png)  

- **门命名：**

   - 根据需求做处理，一般可以不做处理
   - 如果需要处理，仅对Door节点下的模型处理
   - Door命名编码为：楼层-ID, <font color = "#ff0000">TSCENH-K03-F002-2931120</font>
   - **处理后的编码**
  
        ![门编码](http://meetoo.cn:1986/Patch/Image/门编码.png)  

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

 ![内建筑材质](http://meetoo.cn:1986/Patch/Image/内建筑材质.png)  

**选择：**

- 模型根节点

**操作：**

建筑材质的默认路径为：“.../Base/Shared/Materials/BuildingMaterial”,可以不用选择

![建筑材质](http://meetoo.cn:1986/Patch/Image/建筑材质.png)  

默认全部替换，若只修改某个节点下的材质，可以使用**杂类->替换材质**


## 内建筑设置Layer/Tag

![内建筑LayerTag](http://meetoo.cn:1986/Patch/Image/内建筑LayerTag.png)  

**选择：**

- 模型根节点

**操作：**

FloorTag默认为：Building_Floors
Layer默认为：buildingLevels

分别点击设置楼板Tag,设置内建筑Layer并关闭阴影


**注意：** <font color = "#ff0000">内建筑处理完成后，可以标注为Encode,例如TCENH-K03-AR-Encode</font>



## 内建筑导入外建筑打包

![内建筑导入Max层级](http://meetoo.cn:1986/Patch/Image/内建筑导入Max层级.png)  

**选择：**

- 模型根节点

**操作：**

- 内建筑的的层级结构应该调整为正确的编码：

    ![正确层级](http://meetoo.cn:1986/Patch/Image/正确层级.png)  

- 内建筑的Level层级应与外建筑一致
  

## 墙与楼板的映射关系

用于建筑装修，需要知道楼板和墙面对应关系

![墙和楼板的对应关系](http://meetoo.cn:1986/Patch/Image/墙和楼板的对应关系.png)  

**选择：**

- 模型根节点
- 选择数据库

**操作：**


## 楼板新旧模型的映射关系

楼板因为编码的缘故造成以前的编码出现问题，可以使用射线映射的方式，获得新旧楼板的对应关系

![楼板新旧模型的映射关系](http://meetoo.cn:1986/Patch/Image/楼板新旧模型的映射关系.png)  

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

- **管道处理流程**

![管道处理流程](http://meetoo.cn:1986/Patch/Image/管道处理流程.png)  

- **管道模型检查**
    - **假连接检测**
      - 模型连接点是否完整
    - **标高检测**
      - 标高是否准确
    - **系统名称检测**
      - 系统名称是否符合标准，相邻管道系统名称是否一致；
    - **流向检测**
      - 是否因为假连接造成流向断开；

- **管道检测插件**
  
    对应四项检查

    ![Revit插件](http://meetoo.cn:1986/Patch/Image/revit插件.png)  

## 管道替换

![管道替换](http://meetoo.cn:1986/Patch/Image/管道替换.png)  

- 使用Simplygon优化管道模型，可以使用Editor对直管进行替换后，在进行Simplygon优化，
    - 使用Unity绘制Cylinder替换面数复杂的直管
    - 替换之前需要对管道进行按类型划分
    - 暖通系统、给排水系统、医疗气体系统一般分为管道、管件、管道附件、机械设备等；配电系统不需要管道替换；
- 使用PiXYZ进行管道优化的话，不需要对直管进行替换，因为替换后无法保存Mesh;PiXYZ优化的
- 
- 新管道模型替换
    使用Simplygon优化的管道模型导入到Unity后，会有部分管道不显示Mesh,例如冷媒管，需要重新导入，
    用新的模型替换旧的管道

**选择：**

- 模型根节点

**操作：**

- 导入新模型，重新调整新模型的位置
- 选择新的模型模型根节点
- 点击新管道模型替换


## 管道设备剔除

- TODO


## 管道划分

![管道划分](http://meetoo.cn:1986/Patch/Image/管道划分.png)  

- **要求：**

  - Revit基础信息完整；
  - Revit模型楼层参数准确，或者标高参数准确；
  - 


- **管道楼层划分**

    **选择：**

    - 模型根节点
    - 选择数据库

    **操作：**



- **管道子系统划分**

    - Revit系统名称符合项目模型标准；
    - 相连管道系统名称应该保持一致；
    - 项目标准中不存在的系统，应该在项目模型中添加，unity插件做相应更改；

## 管道材质

![管道材质](http://meetoo.cn:1986/Patch/Image/管道材质.png)  

**选择：**

- 模型根节点

**操作：**

- 管道按系统赋予材质
- 旧版材质的路径为：'.../Assets/Base/Shared/Materials/PipeLineMaterial'
- 新版材质的路径为：'.../Assets/Base/Shared/Materials/PipeLineMaterial_New'
- 目前采用旧版材质方式
- 点击管道按系统替换材质

## 管道Layer

![管道Layer](http://meetoo.cn:1986/Patch/Image/管道Layer.png)  

**选择：**

- 模型根节点

**操作：**

- 管道Layer默认为：Pipe
- 就方法设置LOD:管道默认给0.02，配电的默认给0；

## 标记竖管

竖管在楼层展示中应该隐藏，标记出高于本层标高的竖管管道

![标记竖管](http://meetoo.cn:1986/Patch/Image/标记竖管.png)  

- **要求：**
  - 数据库中有竖管参数，竖管参数正确

- **数据库标记竖管**

    **选择：**

    - 模型根节点
    - 选择数据库

    **操作：**

    - 对竖管标记的类型为管道和风管
    - 使用Revit插件标记好竖管参数
    - Revit中将超过本层标高的管道竖管参数标为1-1，本层内的竖管为1，其他非竖管参数为0
    - 竖管标记是通过数据库获取竖管参数中为1-1的ID,增加“#L”标记并隐藏；
    - 选择好模型根节点和数据库后
    - 点击数据库标记竖管即可

- **标高差标记竖管**

    **选择：**

    - 模型根节点，模型根节点为**模型的楼层节点**

    **操作：**

    - 两个参数，一个标高上限，一个标高下限
    - 两个参数需要手动填写，可以选择该楼层中的任意一根管子的高度作为参考
    - 点击标高差标记竖管即可

## 管道编码

![管道编码](http://meetoo.cn:1986/Patch/Image/管道编码.png)  

**选择：**

- 模型根节点

**操作：**

- 管道默认200分到一个节点下；
- 管道编码格式：楼层_系统_ID，例如，BDHCMU-D03-F001_AC_F001;
- 特殊：暖通系统中，水和风系统文件分开做的，避免编码重复：AC-F代表风，AF-W代表水；
  

## 更新数据库

![更新数据库](http://meetoo.cn:1986/Patch/Image/更新数据库.png)  

（1）修改本地数据库表结构，将ID,类型ID字段属性类型改成TEXT,并去除ID的主键；
（2）根据模型的ID,将管道编码、所属系统及管道类型写入数据库
（3）系统名称写入标记中


## 管道终端与房间的映射关系

![管道终端与房间对应关系](http://meetoo.cn:1986/Patch/Image/管道终端与房间对应关系.png)  


# 模型编辑器-设备


- ***设备处理流程图***

    ![设备处理流程](http://meetoo.cn:1986/Patch/Image/设备处理流程.png)  

- ***设备分类及标识***
  
    - **AC**:暖通
    - **PD**:供配电
    - **WSAD**：给排水
    - **MG**:医气
    - **Boiler**:锅炉
  
- ***设备编码***
  
	**编码：** 建筑编码_RevitName
    **例如：** BDHCMU-D03-分集水器 - 5 接口00 标准 [2446080]

- ***设备的层级结构***


    ![设备层级](http://meetoo.cn:1986/Patch/Image/设备层级.png)  



## 设备与房间的映射关系

![设备与房间的映射关系](http://meetoo.cn:1986/Patch/Image/设备与房间的映射关系.png)  


将该设备置于所在房间的节点下；并将映射关系写入Equipment表中

Equipment表结构：从服务器上copy下来，清空并修改表结构，将不为null的去勾选；



## 设备Layer/Collider

![设备LayerCollider](http://meetoo.cn:1986/Patch/Image/设备Layer.png)  

材质：Base/Shared/Material/EquipmentMaterial
Layer：Equipment
碰撞体：MeshCollider


## 设备细分

![设备LayerCollider](http://meetoo.cn:1986/Patch/Image/设备Layer.png)  


## 设备特殊情况

![设备LayerCollider](http://meetoo.cn:1986/Patch/Image/设备Layer.png)  


# 建筑数据库处理

## BIMBuilding

## BIMLevel

## BIMSpace


# 管道数据库处理

将本地Revit导出的DB各表整合到Pipe/PipeType中，PipeIcon是对PipeType的重命名及类型对应的IconName

前提：本地各表中的数据已经更新，ID;类型ID;标记（代表所属子系统）

## Pipe
需要导入的字段：
(1)Code---ID;
(2)bimLevelCode---code前15位;
(3)pipeSubsystem---系统名称;
(4)pipeSubsystemCode---标记值;
(5)pipeTypeCode---类型ID;
(6)serviceLife---默认20;
(7)Size ---尺寸;

## PipeType
需要导入字段：	
(1)Code---ID;
(2)familyName---族名称,
(3)typeName---类型名称

## PipeTypeIconMapping
（1）将所有本地类型表的类型整合到PipeTypeMapping
（2）不同的项目中如果PipeTypeIconMapping中存在共用的部分，不会重复写入；
（3）不同的族类型对应简写的类型
（4）更新PipeType中的PipeIconCode


## PipeIcon
（1）每一种类型有一个Icon对应，公用数据库，各个项目如果PipeIcon存在的类型不会重复写入；
（2）IconName，对应Sprite的名字，通过名字来使用不同的sprite;
（3）IconName为空的类型，需要给一个Name，Name值应该从已有的类似的类型中选择或者UI给新的Icon;


## 数据库检测

1. 检测字段是否符合要求
2. 检测是否有重复项
3. 检测未使用项
4. 检测是否为空或着Null



# 管道流向

需要流向数据的系统：暖通的供回水；新风送风回风排风等；给排水的供回水；配电的控制区域等

前提：需要Revit模型是完整的回路，不存在假连接或者断开的情况；

## PIPE_RELATIONSHIP
流向的最原始表，信息最完整；
相关字段：
（1）PIPE_ID:
	管道Code与模型的命名一致；
	建筑Code+“_”+系统code+“_”+ID
	设备的Code与模型的命名一致;
（2）UPSTREAM: 上游管道
（3）DownStream:下游管道；
（4）TUNNEL:通道值，同一回路的管道Tunnel值相同；
（5）Direction:方向：1-正向；0-逆向
（6）IsValve:是否是阀门：所有带阀字的都是阀门
（7）SUBSYSTEM:所属子系统
（8）SUBSYSTEMCODE:子系统CODE
（9）IsEncodeDevice(弃用)；
（9）IsTerminalPipe：是否是末端管道

## PipeRelation
      数据库中的流向表；
（1）将PIPE_RELAITONSHIP中的PIPE_ID/UPSTREAM/TUNNEL/IsTerminalPipe导入；
           相关字段：
 	pipeCode---PIPE_ID;
	parentCode---UPSTREAM;
	valveTunnelCode---TUNNEL;
	pipeType---IsTerminalPipe;
（2）删除重复项；

## PipeValve
       阀门表
记录阀门所在楼层，打开状态，所属子系统，是否为Tunnel等；
 相关字段：
（1）code---Tunnel值和PIPE_RELATIONSHIP中IsValve = 1的值；
（2）bimlevelCode---code的前15位；
（3）isTunnel---是否位Tunnel
（4）pipeSubsystemCode---所属子系统Code;
（5）阀门状态：默认全部为1,1为打开，0为关闭

## PipeEndBimSpace
通过管道流向中IsTerminalPipe为1的末端管道来获取末端管道与房间的对应关系；
相关字段：
（1）pipeCode:末端管道Code
（2）spaceCode:对应的SpaceCode

## FlowPipeMapping
流向的初始管道ID,包括楼层外和楼层内的
相关字段：
（1）mepSystem:所属MEPsystem
（2）subSystem:所属子系统；
（3）building：所属建筑
（4）level:楼层
（5）pipeCode:初始管道Code
（6）name:建筑外的流向的名字
（7）bidirectionID:双向流向的反向Code
（8）buildingViewFlow:是否为建筑外流向，1为是，0为否


## 注意事项: 
（1）造成回路的管道在获取流向数据时，删除；
（2）获取供水和回水流向时都应该选择初始管道，并且两者的Tunnel值相同
（3）特殊情况，需要根据模型code修改数据库中的流向code


# 设备数据库处理
