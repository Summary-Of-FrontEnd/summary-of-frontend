#  HTML元素类型

html元素一共可以分为三类：块元素、内联元素（行内元素）、内联块状元素。

##  块元素

###  特点

1. 块状元素以块的形式显示为一个矩形区域，默认不设置宽度时，继承父元素的宽度
2. 块元素默认情况下，独占一行，自上而下排列
3. 块元素可以定义自己的宽度和高度，以及盒模型中的任意属性（margin,padding,border）
4. 块元素可以作为一个容器容纳其他的块元素和内联元素

###  常见元素

div	p	ul	ol	li	dl	dt	dd	table	tr	td	form	h1	h2	h3	h4	h5	h6	fieldset

##  内联元素

又称行内元素

###  特点

1. 内联元素也显示为一个矩形区域，但宽高由内容来决定
2. 内联元素默认情况下在一行逐个进行显示
3. 内联元素不能定义自己的宽度和高度，可以定义与宽度相关的属性（如左右margin，左右padding等），不可以定义与高度相关的属性（上下margin，上下padding，line-height等）
4. 内联元素只能嵌套内联元素，不能嵌套块元素

###  常见元素

a	span	strong	b	em	i	label	img	input	textarea	select

##  内联块状元素

###  特点

1. 既有内联元素的特点：在一行逐个进行显示
2. 又具有块元素的特点：可以定义宽高以及盒模型中的任意属性

注：vertical-align属性只针对这类型元素设置有效

##  元素类型转换

可以根据元素样式的display属性对元素进行转换

语法：`display:block|inline-block|inline|none|list-item`

###  display:block

将元素转换成块元素，是大部分块元素的默认display属性值

注：当元素设置了float属性后，就等同于转换为块元素

###  display:inline-block

将元素转换为内联块元素，是img、input这类型元素的默认display属性值

###  display:inline

将元素转换为内联元素，是a、span等内联元素的默认display属性值

###  display:none

将元素隐藏不见

注：`display:none`是将元素整个隐藏不可见，`overflow:hidden`是将溢出部分隐藏不可见，没有溢出的部分正常显示

###  display:list-item

将元素转换为列表项，是li的默认display属性值

##  置换元素和非置换元素

置换元素：浏览器根据元素的标签和属性，来决定元素的具体显示内容

非置换元素：除了置换元素，大部分html元素都是非置换元素，其内容直接显示在浏览器中