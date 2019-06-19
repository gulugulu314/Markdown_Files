---

---

Markdown的基本语法


# 标题
标题常用的用#等表示等级


```
# 2. 一级标题
## 2.1. 二级标题
### 2.1.1. 三级标题
#### 2.1.1.1. 四级标题
```
# 粗体/斜体/高亮
## 斜体
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













