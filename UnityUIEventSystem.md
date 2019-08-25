# EventSystem

## 1、UnityEventSystem

UnityEventSystem有两个组件，EventSystem和StandaloneInputModule

- EventSystem：事件的管理类，主要处理事件的分发、接受等，不仅仅对二维UI做出响应，对三维空间的事件也做出响应
- Standalone Input Module:参数控制类 

EventSystem Standalone Input Mdoule 和Canvas上的Graphic Raycaster图形射线共同控制UI的事件

- Unity的射线包含三类：2D射线，3D射线，和Graphic Raycaster

其中
- 2D射线响应2D的碰撞体，
- 3D射线响应3D的碰撞体，
- Graphic RayCaster响应以Graphic为基类的对象

### 1.2 UI实现事件的方式

- 通过接口的形式：Unity自己定义了很多接口，例如IDragHandler,IPointerClickHandler等，UI直接继承该接口，并实现接口的方法，即可
- 通过组件的形式实现：添加EventTrigger组件；
- 通过代码添加


### 1.3 接口

- 拖拽类接口 
- **IInitializaPotentialDragHandler**
- **IBeginDragHandler**
- **IDragHandler**
- **IEndDragHandler**

其他三个接口必须依赖IDragHandler，必须有这个接口存在，才能执行;

还有另一个拖拽的接口

- **IDropHandler**

可以理解为放下的接口，IDropHnadler在IEndDragHandler之后执行；IDropHandler接口也是依赖IDragHandler的；

遇到的问题，是层级问题，两个UI覆盖时，加一个canvas和graphic raycaster，并且覆盖


点击事件的接口
- **IPointerEnterHandler**
- **IPointerExitHandler**
- **IPointerDownHandler**
- **IPointerUpHandler** 
- **IPointerClickHandler**


选中状态的接口
- **ISelectHandler**
- **IDSelectHandler**
- **IUpdateSelectHandler**

要实现接口，必须有selectable组件

# UI组件的扩展