#  position属性

CSS中的position属性用于指定一个元素在文档中的定位方式。可以使用top、right、bottom、left属性确定元素的最终位置。

##  属性值

- static

  **默认值**。没有定位，元素出现在正常的流中（忽略top、right、bottom、left或z-index声明）

- relative

  生成相对定位的元素，相对于其正常位置（未添加定位时的位置）进行定位

- absolute

  生成绝对定位的元素，相对于**static定位以外的第一个父元素**进行定位

- fixed

  生成绝对定位的元素，相对于浏览器窗口进行定位

- inherit

  规定从父元素继承position属性的值



#  float属性

使元素向左或向右移动，其周围的元素也会重新排列

元素只能水平方向浮动，也就是只能左右移动

元素未设置浮动时按照标准流显示，当设置了float属性则可以让该元素脱离标准流，剩余元素自动填补其位置。

**注意**：假如某个div元素A是浮动的，如果A元素上一个元素也是浮动的，那么A元素会跟随在上一个元素的后边(如果一行放不下这两个元素，那么A元素会被挤到下一行)；如果A元素上一个元素是标准流中的元素，那么A的相对垂直位置不会改变，也就是说A的顶部总是和上一个元素的底部对齐。

使用clear属性可以清除元素的浮动

属性：

- left：元素向左浮动
- right：元素向右浮动
- none：默认值。元素不浮动，并会显示在其在文本中出现的位置
- inherit：从父元素处继承float属性的值



#  水平垂直居中

##  水平居中

CSS设置水平居中较为简单

- 块级元素设置margin:0 auto（注意：得先设置元素width属性）
- 内联元素设置父元素text-align:center

![image-20200525164227181](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200525164227181.png)

![1](E:\前端\HTML\1.png)

##  垂直居中

可以通过设置position属性，使元素移动

只需要设置top:50%(垂直) left:50%(水平)

但此时只是最上方和最左方处于中间处，所以需要再调整

当知道元素长宽时，只要将其向上移动一半宽度的像素，向左移动一半长度的像素

![image-20200525170156331](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200525170156331.png)

![2](E:\前端\HTML\2.png)



如果不知道元素的长宽，则可以通过设置元素的transform:translateY(-50%)，意思是使元素向上平移自身高度的一半

![image-20200525171435524](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200525171435524.png)



还可以将布局改为弹性布局

![image-20200525171829458](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200525171829458.png)





#  盒模型

所有HTML元素可以看作盒子。盒模型可以用来对元素进行布局，包括内边距，边框，外边距和实际内容的这几个部分。

![CSS box-model](https://www.runoob.com/images/box-model.gif)

- margin（外边距）：由外边距边界限制，用空白区域扩展边框区域，以分开相邻的元素
- border（边框）：由边框边界限制，扩展自内边距区域，是容纳边框的区域
- padding（内边距）：由内边距边界限制，扩展自内容区域，负责延伸内容区域的背景，填充元素中内容与边框的间距
- content（内容）：由内容边界限制，容纳着元素的“真实”内容，例如文本、图像，或是一个视频播放器

##  盒子大小的计算

总元素的宽度=宽度+左填充+右填充+左边框+右边框+左边距+右边距

总元素的高度=高度+顶部填充+底部填充+上边框+下边框+上边距+下边距





#  选择器

##  类型

- 标签选择器（类型选择器）

- 类选择器

  通过设置元素的 `class` 属性，可以为元素指定类名。多个元素可拥有同一个类名

  样式表中，以(.)开头

- ID选择器

  通过设置元素的`id`属性为该元素制定ID，且ID在一个文档中是唯一的

  样式表中，以(#)开头

- 伪类选择器（Pseudo-classes selectors）

  定义：伪类是加在选择器后面用来指定元素状态的关键字，为选择器添加状态。用于选择不存在于DOM树中的信息或那些不能够通过常规css选择器得到的信息。

- 全局选择器（以(*)开头）

- 属性选择器（样式表中，以[key=value]开头）

注意：优先度（确定度）：内联样式>ID选择器>伪类/类选择器>标签选择器>通配选择器

##  各种各样的关系

| **选择器**      | **选择的元素**                                               |
| --------------- | ------------------------------------------------------------ |
| `A E`           | 元素A的任一后代元素E (后代节点指A的子节点，子节点的子节点，以此类推) |
| `A > E`         | 元素A的任一子元素E(也就是直系后代)                           |
| `E:first-child` | 任一是其父母结点的第一个子节点的元素E                        |
| `B + E`         | 元素B的任一下一个兄弟元素E                                   |
| `B ~ E`         | B元素后面的拥有共同父元素的兄弟元素E                         |





#  布局种类

- 静态布局（Static Layout）

  特点：网页上所有元素的尺寸一律使用px作为单位，不管浏览器尺寸具体是多少，页面布局始终按照最初写代码时的布局来显示。

- 流式布局（Liquid Layout）

  特点：页面元素的宽度按照屏幕分辨率进行适配，屏幕分辨率变化时，页面内元素大小会变化但布局不变

  设计方法：使用%百分比定义宽度，高度大都用px来固定

- 自适应布局（Adaptive Layout）

  特点：分别为不同的屏幕分辨率定义布局，即创建多个静态布局，每个静态布局对应一个屏幕分辨率范围，页面元素不随窗口大小的调整发生变化

  设计方法：使用 @media 媒体查询给不同尺寸和介质的设备切换不同的样式。在优秀的响应范围设计下可以给适配范围内的设备最好的体验，在同一个设备下实际还是固定的布局。

- 响应式布局（Responsive Layout）

  特点：每个屏幕分辨率下都会有一个布局样式，即元素位置和大小都会改变

  设计方法：媒体查询+流式布局。通常使用 @media 媒体查询 和网格系统 (Grid System) 配合相对布局单位进行布局，实际上就是综合响应式、流动等上述技术通过 CSS 给单一网页不同设备返回不同样式的技术统称。

- 弹性布局（rem/em布局）

  特点：通过调整其内元素的宽高，从而在任何显示设备上实现对可用显示空间最佳填充能力，包裹文字的各元素的尺寸采用em/rem做单位，使用 em 或 rem 单位进行相对布局，相对%百分比更加灵活。

  rem：相对于根元素<html>来设置字体大小

  em：以使用em为单位的元素本身的font-size大小为参考点

比较

|            | 优点                                                         | 缺点                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 静态布局   | 编写简单，不存在兼容性问题                                   | 不能根据用户的屏幕尺寸做出不同的表现                         |
| 流式布局   | 可以应对不同尺寸的PC屏幕                                     | 宽度使用百分比定义，但是高度和文字大小等大都是用px来固定，在大屏幕的手机下显示效果会不协调 |
| 自适应布局 | 在优秀的响应范围设计下可以给适配范围内的设备最好的体验，在同一个设备下实际还是固定的布局。 |                                                              |
| 响应式布局 | 适应pc和移动端，如果足够耐心，效果完美                       | 媒体查询是有限的，也就是可以枚举出来的，只能适应主流的宽高。<br />要匹配足够多的屏幕大小，工作量不小，设计也需要多个版本。 |
| 弹性布局   | 理想状态是所有屏幕的高宽比和最初的设计高宽比一样，或者相差不多，完美适应 | rem+js只不过是宽度自适应，高度没有做到自适应，一些对高度，或者元素间距要求比较高的设计，则这种布局没有太大的意义。如果只是宽度自适应，更推荐响应式设计。 |













#  布局样式

##  三栏布局

要求：一般多指左右两栏宽度固定，中间栏宽度自适应的布局



![image-20200603162210388](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200603162210388.png)

###  利用绝对定位

直接定位置

HTML

```html
<div class="container">
        <div class="main">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Dolore sapiente adipisci ullam quo dolor recusandae modi, iste quidem non reiciendis minus quia numquam aliquid. Vitae dolores dignissimos laboriosam nobis cupiditate!</div>
        <div class="left">左侧栏</div>
        <div class="right">右侧栏</div>
    </div>
```

CSS

```css
.left{
    position: absolute;
    width: 100px;
    left: 0;
    top: 0;
    background: aqua;
}
.right{
    position: absolute;
    width: 200px;
    right: 0;
    top: 0;
    background: red;
}
.main{
    position: absolute;
    left: 100px;
    right: 200px;
    top: 0;
    background: lightpink;
}
```

特点：

- 简单、粗暴
- 采用了绝对定位（absolute），导致父元素脱离文档流，若页面复杂，开发难度巨大

###  利用浮动

HTML

```html
<div class="container">
    <div class="left">左侧栏</div>
    <div class="right">右侧栏</div>
    <div class="main">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Dolore sapiente adipisci ullam quo dolor recusandae modi, iste quidem non reiciendis minus quia numquam aliquid. Vitae dolores dignissimos laboriosam nobis cupiditate!</div>
</div>
```

CSS

```css
.left{
    width: 100px;
    float: left;
    background: aqua;
}
.right{
    width: 200px;
    float: right;
    background: red;
}
.main{
    background: lightpink;
    overflow: hidden;
}
```

特点：

- 简单、兼容性好
- 当中间内容高于两侧时，两侧高度不会随中间内容变高而变高

###  利用弹性盒子布局

![image-20200603163359841](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200603163359841.png)

HTML

```htm
<div class="container">
<div class="left">左侧栏</div>
<div class="main">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Dolore sapiente adipisci ullam quo dolor recusandae modi, iste quidem non reiciendis minus quia numquam aliquid. Vitae dolores dignissimos laboriosam nobis cupiditate!</div>
<div class="right">右侧栏</div>
</div>
```

CSS

```css
.container{
    display: flex;
}
.left{
    flex: 0 0 100px;
    background: aqua;
}
.right{
    flex: 0 0 200px;
    background: red;
}
.main{
    background: lightpink;
    flex: 1;
}
```

特点：

- 简单、优雅
- 未来趋势
- 可在移动端尽情使用
- 兼容性差一点，不支持 IE8、IE9

###  利用负边距和浮动





##  圣杯布局

要求：

- 上部（header）和下部（footer）各自占领屏幕所有宽度
- 上下部之间的部分（container）是一个三栏布局
- 三栏布局两侧宽度不变，中间部分自动填充整个区域
- 中间部分的高度是三栏中最高的区域高度

![图片描述](https://segmentfault.com/img/bVblLcC?w=1664&h=380)

###  实现方法：浮动

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .header, .footer {
            border: 1px solid #333;
            background: #ccc;
            text-align: center;
        }
        .footer {
            clear: both;
        }

        .container {
            padding:0 220px 0 200px;
            overflow: hidden;
        }
        .left, .main, .right {
            position: relative;
            float: left;
            min-height: 130px;
        }
        .main {
            width: 100%;
            background: blue;
        }
        .left {
            margin-left: -100%;
            left: -200px;
            width: 200px;
            background: red;
        }
        .right {
            margin-left: -220px;
            right: -220px;
            width: 220px;
            background: green;
        }
    </style>
</head>
<body>
    <div class="header">
        <h4>header</h4>
    </div>
    <div class="container">
        <div class="main">
            <h4>middle</h4>
            <p>middle-content</p>
        </div>

        <div class="left">
            <h4>left</h4>
            <p>left-content</p>
        </div>

        <div class="right">
            <h4>right</h4>
            <p>right-content</p>
        </div>
    </div>
    <div class="footer">
        <h4>footer</h4>
    </div>
</body>
</html>

```

###  实现方法：flex弹性盒子

只需要将中间部分设置为flex布局即可

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .header, .footer {
            border: 1px solid #333;
            background: #ccc;
            text-align: center;
        }
        .container {
           display: flex;
        }
        .left, .main, .right {
            min-height: 130px;
        }
        .main {
            flex: 1;
            background: blue;
        }
        .left {
            flex: 0 0 200px;
            background: red;
        }
        .right {
            flex: 0 0 200px;
            background: green;
        }
    </style>
</head>
<body>
    <div class="header">
        <h4>header</h4>
    </div>
    <div class="container">
        <div class="left">
            <h4>left</h4>
            <p>left-content</p>
        </div>
        <div class="main">
            <h4>middle</h4>
            <p>middle-content</p>
        </div>
        <div class="right">
            <h4>right</h4>
            <p>right-content</p>
        </div>
    </div>
    <div class="footer">
        <h4>footer</h4>
    </div>
</body>
</html>

```

left和right区域定宽，middle设置flex:1即可





#  BFC

##  前言

控制语速布局的定位方案常见有三种：

- 普通流
- 浮动
- 绝对定位

##  概念

Formatting Context，又名格式化上下文，是页面中的一块渲染区域，并且有一套渲染规则，决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

BFC即Block Formatting Context（块级格式化上下文），**具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。**

##  布局规则

- 内部的Box会在垂直方向一个接一个地放置
- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
- 每个盒子（块盒与行盒）的margin box的左边，与包含border box的左边相接触。即使存在浮动也是如此。
- BFC的区域不会与float box重叠
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然
- 计算BFC高度时，浮动元素也参与计算

##  创建

一个块格式化上下文由以下之一创建：

- 根元素或其他包含它的元素
- 浮动元素（元素的float不是none）
- 绝对定位元素（元素具有position为absolute或fixed）
- 内联块（元素具有display:inline-block）
- 表格单元格（元素具有display:table-cell，HTML表格单元默认属性）
- 表格标题（元素具有display:table-caption，HTML表格标题默认属性）
- 具有overflow且值不为visible的块元素
- **display:flow-root**
- column-span:all应当总是会创建一个新的格式化上下文，即便具有column-span:all的元素并不包裹在一个多列容器中
- 一个快格式化上下文包括创建它的元素内部所有内容，除了被包含于创建新的块级格式化上下文的后代元素内的元素

##  功能特性

- 利用BFC避免margin重叠

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190323160150540.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM2NDIyMjM2,size_16,color_FFFFFF,t_70)

- 自适应两栏布局（BFC的区域不会和float box重叠）

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032316073845.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM2NDIyMjM2,size_16,color_FFFFFF,t_70)

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190323161159873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM2NDIyMjM2,size_16,color_FFFFFF,t_70)

- 清除浮动（父节点不设置高度，子节点设置浮动时发生高度塌陷）

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190323161720931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM2NDIyMjM2,size_16,color_FFFFFF,t_70)

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190323161945489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM2NDIyMjM2,size_16,color_FFFFFF,t_70)











#  页面布局

定义：对页面的文字、图形或表格进行格式设置。包括字体、字号、颜色纸张大小和方向以及页边距等。

##  Flex布局

定义：Flexible Box，意为“弹性布局”，用来为盒装模型提供最大的灵活性（需用display指定）。

注意：设为Flex布局后，子元素的float、clear和vertical-align属性将失效。

优点：容易上手，使用就简单

缺点：浏览器兼容性不算太好，只能兼容到ie9及以上

###  基本概念

采用Flex布局的元素，称为Flex容器，其所有子元素自动成为容器成员。

容器默认存在两根轴：水平主轴（main axis）和垂直的交叉轴（cross axis）。

项目默认沿主轴排列。单个项目占据的主轴空间叫作main size，占据的交叉轴空间叫作cross size。

###  容器的属性

- flex-direction：决定主轴的方向
  - row（默认值）：主轴为水平方向，起点在左端
  - row-reverse：主轴为水平方向，起点在右端
  - column：主轴为垂直方向，起点在上沿
  - column-reverse：主轴在垂直方向，起点在下沿
- flex-wrap：定义在一条轴排列不下的情况下如何换行
  - nowrap（默认）：不换行
  - wrap：换行，第一行在上方
  - wrap-reverse：换行，第一行在下方
- flex-flow：是flex-direction和flex-wrap的简写形式
- justify-content：定义了项目在主轴上的对齐方式
  - flex-start：靠开始端对齐
  - flex-end：靠结束端对齐
  - center：居中
  - space-between：两端对齐，项目之间的间隔都相等
  - space-around：每个项目两侧间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍
- align-items：定义项目在交叉轴(y轴)上如何对齐
  - flex-start：交叉轴的起点对齐
  - flex-end：交叉轴的终点对齐
  - center：交叉轴的中点对齐
  - baseline：项目的第一行文字的基线对齐
  - stretch（默认）：如果项目未设置高度或设为auto，将占满整个容器的高度
- align-content：定义了多根轴线的对齐方式。若项目只有一根轴线，该属性不起作用
  - `flex-start`：与交叉轴的起点对齐。
  - `flex-end`：与交叉轴的终点对齐。
  - `center`：与交叉轴的中点对齐。
  - `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
  - `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
  - `stretch`（默认值）：轴线占满整个交叉轴。

###  项目的属性

- order：定义项目的排列顺序。数值越小，排列越靠前，默认为0
- flex-grow：定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
- flex-shrink：定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小（不能为负值）
- flex-basis：定义了在分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。可以用于设置固定空间
- flex：`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选（优先使用）
- align-self：允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`







#  CSS3

##  CSS Transform

定义：Transform指代变形，指旋转rotate、扭曲skew、缩放scale、移动translate和矩阵变形matrix

##  CSS Transition

定义：transition是一个复合属性，用来使样式的改变平滑的过度，可以让开发人员不需要使用JavaScript就可以实现简单的动画交互效果。包括transition-property、transition-duration、transition-timing-function、transition-delay四个属性。

###  属性

- transition-property：指定可过渡样式
  - none：没有指定任何样式
  - all：默认值，表示指定元素所有支持transition-property属性的样式
  - transition-property：可过渡的样式（若没设置，其他样式立即改变）
- transition-duration：指定过渡持续时间，必须带单位
  - time：初始值为0s，不能为负，若有多个值，则过渡属性按照顺序对应持续时间
- transition-delay：定义元素属性延迟多少时间后开始过渡效果，必须带单位
  - 该属性若为负值，无延迟效果，但过渡元素的起始值将从0变成设定值(设定值=延迟时间+持续时间)。若该设定值小于等于0，则无过渡效果；若该设定值大于0，则过渡元素从该设定值开始完成剩余的过渡效果
  - 若有多个值，则过渡属性按照顺序对应持续时间
- transition-timing-function：定义元素过渡属性随时间变化的过渡速度变化效果（可有多个值）
  - timing-function
    - 关键字：
      - ease：开始结束慢，中间快
      - linear：匀速
      - ease-in：开始慢
      - ease-out：结束慢
      - ease-in-out：与ease类似，但幅度更大
      - step-start：直接位于结束处
      - step-end：位于开始处经过时间间隔后结束
    - steps函数：将过渡时间划分成大小相等的时间时隔来运行
    - bezier函数：通过p0-p3四个控制点来控制，其中p0表示(0,0)，p3表示(1,1)

注意：

- 只有拥有中间状态的样式可以设置transition
- 需要事件触发，无法在网页加载时自动发生
- 一次性，不能重复发生，除非再次触发
- 每次只能定义一个属性

##  CSS Animation

定义：用来设计样式改变的动画（必须放在选择器中）

组成：

- 关键帧：@keyframes，定义动画在不同阶段的状态
- 动画属性：决定动画的播放时长，播放次数，以及用何种函数式去播放动画等
- css属性：css元素在不同关键帧下的状态

语法：

```
.element {
    animation: [name][duration][timeing-function][delay][iteration-count][direction][fill-mode][play-state];
    }
```



###  关键帧

keyframes用来定义动画的各个状态

语法形如：

```
@keyframes keyframes-name {
from {
    [styles];
       }
  to {
    [styles];
     }
}
keyframes-name
帧列表的名称。 名称必须符合 CSS 语法中对标识符的定义。
from
等效于 0%.
to
等效于 100%.
```

###  动画属性

- animation-timing-function：时间函数，定义了动画的播放速度曲线
- animation-direction：动画方向，表示CSS动画是否反向播放
- animation-delay：动画延迟，定义动画是从何时开始播放
- animation-iteration-count：动画迭代次数，定义动画播放的次数
- animation-fill-mode：动画填充模式，指给定动画播放前后应用元素的样式
- animation-timing-function：动画播放状态，定义动画是否运行或暂停





#  CSS预处理器

CSS是面向命名语言，无法定义变量以及没有合理的样式复用机制导致整体CSS样式难以维护，可维护性低。

##  概念

CSS预处理器用一种专门的编程语言，进行Web页面样式设计，然后再编译成正常的CSS文件，以供项目使用。

##  种类

- Sass：2007年诞生，最早也是最成熟的CSS预处理器，拥有ruby社区的支持和compass这一最强大的CSS框架，目前受LESS影响，已经进化到了全面兼容CSS的SCSS。
- Less：2009年出现，受Sass的影响较大，在ruby社区之外支持者远超Sass，其缺点是比起Sass来，编程功能不够，不过优点是简单和兼容CSS。
- Stylus：2010年产生，来自Node.js社区，主要用来给Node项目进行CSS预处理支持，但不比另外两个普及。

##  相同点

可以在CSS中使用变量、简单的程序逻辑、函数等在编程语言中的一些基本技巧，可以让CSS更简洁，适应性更强，代码更直观等。

##  不同点

###  变量

- Sass声明变量必须是"$"开头，后面紧跟变量名和变量值，而且两者之间需用冒号:分隔开
- LESS变量名必须以“@”开头，变量名和变量值之间以冒号:隔开
- Stylus对变量名没有任何限定，变量名与变量值之间可以用冒号、空格和等号隔开

###  作用域

三种预处理器中定义的变量都是有作用域的，查找变量的顺序是先在局部定义中查找，如果找不到，则逐级向上查找。（与JS一样）

如果我们在代码中重写某个已经定义的变量，Less的处理逻辑和其他两个有区别。Less中，这个行为叫懒加载（Lazy Loading）。**注意**Less中所有变量的计算，都是以这个变量最后一次被定义的值为准。

Stylus和SASS行为相同，变量的计算以变量最近一次的定义为准。

Less

```less
@size: 40px;
.content {
    width: @size;
}
@size: 60px;
.container {
    width: @size
}
```

编译为

```css
.content {
    width: 60px;
}
.container {
    width: 60px;
}
```

Sass

```scss
$size: 40px;
.content {
    width: $size;
}
$size: 60px;
.container {
    width: $size;
}
```

编译为

```css
.content {
    width: 40px;
}
.container {
    width: 60px;
}
```

###  嵌套

如果需要在相同的父元素中选择多个子元素，需要一遍又一遍地写父元素，如果用CSS预处理器就可以不用重复写父元素，并且父元素和子元素的关系一目了然。

SASS还提出了属性嵌套，属性嵌套指的是有些属性拥有相同的开始单词，如border-width，border-color都是以border开头。

###  继承

- Sass和Stylus的继承很像，能把一个选择器的所有样式继承到另一个选择器上。使用“@extend”开始，后面接被继承的选择器
- Less继承与另外两者有所区别，它是将Mixins中的样式嵌套到每个选择器里面。但具有选择器样式重复的特性





#  Bootstrap

##  栅格系统

Bootstrap提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列（column）。

###  工作原理

- “行（row）”必须包含在`.container`（固定宽度）或`.container-fluid`（100%宽度）中，以便为其赋予合适的排列与内补
- 通过“行（row）”在水平方向创建一组“列（column）”
- 内容只能放置在“列（column）”内，并且**只有**“列（column）”可以作为“行（row）”的直接子元素
- 通过预先定义的类实现栅栏布局
- 通过为“列（column）”设置 `padding` 属性，从而创建列与列之间的间隔（gutter）。通过为 `.row` 元素设置负值 `margin` 从而抵消掉为 `.container` 元素设置的 `padding`，也就间接为“行（row）”所包含的“列（column）”抵消掉了`padding`
- 通过媒体查询确定`.container`的宽度，将最外面的布局元素 `.container` 修改为 `.container-fluid`，就可以将固定宽度的栅格布局转换为 100% 宽度的布局
- 超小屏幕`.col-xs`，小屏幕`.col-sm`，中屏幕`.col-md`，大屏幕`.col-lg`
- `.row`元素每一行被分为12列，如果在一个 `.row` 内包含的列（column）大于12个，包含多余列（column）的元素将作为一个整体单元被另起一行排列

**特别注意**：

- 将每一个`.row`分为12列是通过设置每一列的宽度`width`百分比
- **列偏移**`.col-md-offset-*`类可以将列向右偏移，通过设置左侧边距（margin-left）实现
- **列排序**通过使用 `.col-md-push-*` 和 `.col-md-pull-*` 类就可以很容易的改变列（column）的顺序。 `.col-md-push-*`设置到左边的距离，`.col-md-pull-*`设置距离右边的距离