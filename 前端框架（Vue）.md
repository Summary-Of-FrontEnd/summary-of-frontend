#  Vue

**官方解释**：Vue 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注**视图层**，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

Vue的核心功能就是一个视图模板引擎，强调的是状态到界面的映射。此时的Vue并不能被称为框架，只有在添加组件系统、客户端路由、大规模状态管理后才能成为框架（渐进式）。

##  基本原理

![Vue的原理图](https://segmentfault.com/img/bV9lca?w=1200&h=750)

1. 建立虚拟DOM树，通过document.createDocumentFragment()，遍历指定根节点内部节点，根据{{prop}}、v-model等规则进行compile；
2. 通过Object.defineProperty()进行数据变化拦截；
3. 截取到的数据变化，通过发布者—订阅者模式，触发Watcher，从而改变虚拟DOM中的具体数据；
4. 通过改变虚拟DOM元素值，从而改变最后渲染dom树的值，完成双向绑定。



##  Vue的虚拟DOM

### 前言

#### 模板转换视图过程

1. Vue.js通过编译将template模板转换成渲染函数render，执行渲染函数就可以得到一个虚拟节点树
2. 在对Model进行操作时，会触发对应Dep中的Watcher对象。Watcher对象会调用对应的update来修改视图。通过对比差异，根据对比结果进行DOM操作来更新视图

![img](https://image.fundebug.com/2019-06-26-01.png)

- 渲染函数：渲染函数主要用来生成Virtual DOM。在Vue中使用模板构建应用界面，在底层实现中，Vue会将模板编译成渲染函数render。
- VNode虚拟节点：是一个节点描述对象，可以表示为一个真实的DOM节点。通过createElement方法能将VNode渲染成DOM节点。
- patch：虚拟DOM的最核心部分，可以将VNode渲染成真实DOM，会对比新旧虚拟节点之间有何不同，然后根据对比结果找出需要更新的节点进行更新。



### 概念

Virtual DOM其实就是一棵以**JavaScript对象（VNode节点）作为基础的树**，用对象属性来描述节点，实际上它只是一层对真实DOM的抽象。最终可以通过一系列操作使这棵树映射到真实环境上。简单来说，可以把Virtual DOM 理解为一个简单的JS对象，并且最少包含**标签名**( tag)、**属性**(attrs)和**子元素对象**( children)三个属性。不同的框架对这三个属性的命名会有点差别。

### 作用

1. 提供与真实DOM节点所对应的虚拟节点VNode
2. 将虚拟节点VNode和旧虚拟节点oldVNode进行对比，然后更新视图

###  优点（为什么）

- 具备跨平台的优势：由于虚拟DOM是以JavaScript对象为基础而不依赖平台环境，所以具备了跨平台的能力
- 运行快，效率高：操作DOM的执行效率低，因此将DOM操作放在JavaScript中运用patching算法找出真正需要更新的节点，减少DOM操作，提高性能
- 提升渲染能力：可以多次重复渲染，实现对识图进行合理、高效的更新

###  diff算法

两个假设：

1. 两个相同组件产生类似的DOM结构，不同的组件产生不同的结构
2. 对于同一层次的一组子节点，他们可以通过唯一的id进行区分

基于snabbdom改造过来的，仅在同级的VNode间做diff，递归地进行同级VNode的diff，最终实现整个DOM树的更新（同层次节点对比，深度优先）。

![img](https://upload-images.jianshu.io/upload_images/1828354-a2610ed017b10b50.png?imageMogr2/auto-orient/strip|imageView2/2/w/736/format/webp)

1. 标签名变更，则整个节点统一进行替换，里面的子节点也跟着替换
2. 标签的属性变更，把变更的属性应用上去
3. 文本节点内容变更，直接替换即可
4. 子元素个体的增加、删除、移动

步骤：

1. 用JavaScript对象结构表示DOM树的结构；然后用这个数构建一个真正的DOM树，插入文档中；
2. 当状态变更时，重新构造一棵新的对象树，然后用新的树和旧的树进行比较，记录两棵树的差异
3. 把所**记录的差异**(patch)应用到所构建的真正的DOM树上，视图实现更新。

![img](https://image.fundebug.com/2019-06-26-04.png)



##  Vue的双向绑定原理

###  双向绑定的实现要点

1. 实现一个数据监听器Observer，能够对数据对象的所有属性进行监听，若有变动可获取最新值并通知订阅者；
2. 实现一个指令解析器Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数；
3. 实现一个Watcher，作为连接Observer和Compile的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图；
4. MVVM入口函数，整合以上三者。

###  Vue双向绑定原理

Vue的双向绑定是由**数据劫持**结合**发布者—订阅者模式**实现的。

####  数据劫持

Vue是通过Object.defineProperty()来劫持对象属性的setter和getter。

> 语法：Object.defineProperty(obj,prop,descriptor)
>
> 参数：
>
> ​	obj：要在其上定义属性的对象
>
> ​	prop：要定义或修改的的属性的名称
>
> ​	descriptor：将被定义或修改的属性描述符
>
> 返回值：被传递给函数的对象

```js
//在console.log(book.name)同时,直接给书加一个书号
var Book = {};
var name = '';
Object.defineProperty(Book,'name',{
    set:function(value) {
        name = value;
        console.log('你取了一个书名叫:'+value);
    },
    get:function() {
        console.log('get方法被监听到');
        return '<'+name+'>';
    }
});
Book.name = '人性的弱点';  //set     result：你取了一个书名叫:人性的弱点
console.log(Book.name);　//get     result：<人性的弱点>
```

####  发布者—订阅者模式

**概念**：定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变，所有依赖于它的对象都将得到通知。

**步骤**：

```undefined
1、初始化发布者、订阅者。
2、订阅者需要注册到发布者，发布者发布消息时，依次向订阅者发布消息。
```

![img](https://upload-images.jianshu.io/upload_images/12653633-345cc3e0ada0e06c.png?imageMogr2/auto-orient/strip|imageView2/2/w/700/format/webp)

![img](https://upload-images.jianshu.io/upload_images/12653633-9d69ec0f1ef76181.png?imageMogr2/auto-orient/strip|imageView2/2/w/700/format/webp)

###  Vue双向绑定具体实现

**思路分析**：![img](https://upload-images.jianshu.io/upload_images/12653633-fa21a82839e175a7.png?imageMogr2/auto-orient/strip|imageView2/2/w/581/format/webp)

####  实现过程

我们已经知道如何实现数据的双向绑定了，那么首先要对数据进行劫持监听，所以我们先要设置一个监听器Observer,用来监听所有的属性，当属性变化时，就需要通知订阅者Watcher,看是否需要更新．因为属性可能是多个，所以会有**多个订阅者**，故我们需要一个**消息订阅器Dep**来专门收集这些订阅者，并在监听器Observer和订阅者Watcher之间进行**统一的管理**．因为在节点元素上可能存在一些指令，所以我们还需要有一个指令解析器Compile，对每个节点元素进行扫描和解析，将相关指令初始化成一个订阅者Watcher，并替换模板数据并绑定相应的函数，这时候当订阅者Watcher接受到相应属性的变化，就会执行相对应的更新函数，从而更新视图．

整理上面的思路，我们需要实现三个步骤，来完成双向绑定：

```undefined
1.实现一个监听器Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者。
 
2.实现一个订阅者Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。
 
3.实现一个解析器Compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的订阅器。
```

流程图如下：

![img](https:////upload-images.jianshu.io/upload_images/12653633-d82f70569b8c385f.png?imageMogr2/auto-orient/strip|imageView2/2/w/785/format/webp)

#####  **Observer**

数据监听器的核心方法就是Object.defineProperty( )，通过遍历循环对所有属性值进行监听，并对其进行Object.defineProperty( )处理.另外，我们还需要一个可以容纳消息订阅者的消息订阅器Dep，订阅器主要收集消息订阅者，然后在属性变化时执行相应订阅者的更新函数，那么消息订阅器Dep需要有一个容器，用来存放消息订阅者．

```js
function defineReactive(data,key,val) {
    observe(val);
    var dep = new Dep();
    Object.defineProperty(data, key, {
        enumerable: true,
        configurable: true,
        get: function() {
            if (是否需要添加订阅者) {    //Watcher初始化触发
                dep.addSub(watcher); // 在这里添加一个订阅者
            }
            return val;
        },
        set: function(newVal) {
            if (val === newVal) {
                return;
            }
            val = newVal;
            console.log('属性' + key + '已经被监听了，现在值为：“' + newVal.toString() + '”');
            dep.notify(); // 如果数据变化，通知所有订阅者
        }
    });
}
 
function observe(data) {
    if(!data || typeof data !== 'object') {
        return;
    }
    Object.keys(data).forEach(function(key){
        defineReactive(data,key,data[key]);
    });
}
 
function Dep() {
    this.subs = [];
}
 
//prototype 属性使您有能力向对象添加属性和方法
//prototype这个属性只有函数对象才有，具体的说就是构造函数具有.只要你声明定义了一个函数对象，这个prototype就会存在
//对象实例是没有这个属性
Dep.prototype = {                        
    addSub:function(sub) {
        this.subs.push(sub);
    },
    notify:function() {
        this.subs.forEach(function(sub) {
            sub.update();        //通知每个订阅者检查更新
        })
    }
}
Dep.target = null;
```

#####  **Watcher**

…………………………





#  Vue指令

Vue指令是一种特殊的自定义行间属性（在html标签内写），职责就是当其表达式的值改变时相应的将某些行为应用到DOM上。

- v-bind：动态绑定数据
- v-on：绑定事件
- v-text：更新数据，会覆盖已有结构。类似{{msg}}
- v-show：根据值的真假，切换元素的display属性
- v-if：根据值的真假，切换元素会被销毁、重建 =》在DOM中已消失
- v-for：基于源数据多次渲染或模块
- v-model：在表单控件元素上创建双向数据绑定（语法糖，原理如下）
  1. 绑定了DOM标签的input事件
  2. 当用户进行输入时，触发事件函数，事件函数内部读取此DOM标签的value值，赋给vue实例
- v-once：只渲染一次，最后数据更新也不重新渲染









# Vue的生命周期

定义：Vue实例从开始创建、初始化数据、编译模板、挂载DOM并渲染、更新并渲染和卸载等一系列过程。

在Vue实例初始化的过程中也会运行八个生命周期函数，可以按需求在各个阶段添加代码。

![img](https://upload-images.jianshu.io/upload_images/13119812-eaf493b1b2050a93.png?imageMogr2/auto-orient/strip|imageView2/2/w/720/format/webp)

- beforeCreate()    创建前
- created()    创建
- beforeMount()    挂在前
- mounted()    挂载
- beforeUpdate()    更改前
- updated()    更改
- beforeDestroy()    销毁前
- destroyed()    销毁

##  beforeCreate

实例初始化在这个生命周期遍历data对象下所有属性，将其转化为getter/setter，也就是在这个时候添加Observer。此时组件里的属性还不存在。

##  created

实例创建完毕，属性已绑定且能够操作，会把template模板编译成html和执行render函数，返回一个虚拟DOM，但DOM还不存在。已可以执行简单ajax/axios请求。

##  beforeMount

找到对应的template，并编译成render函数。检查是否有任何模板可用于要在DOM中呈现的对象。如果没有找到模板，那么它将所定义元素的外部HTML视为模板。

##  mounted

将实例挂载到DOM上，完成虚拟DOM到真实DOM的渲染。

***以上四个生命钩子在每个组件中只执行一次***

##  beforeUpdate

响应式数据更新时调用，发生在虚拟DOM打补丁之前。

##  updated

虚拟DOM重新渲染和打补丁之后调用，此时组件已经更新，可执行依赖于DOM的操作（避免在这个钩子函数中操作数据，不然可能陷入死循环）。

##  beforeDestroy

实例销毁前调用，此时还可以获取实例。

##  destroyed

实例销毁后调用，调用后，Vue实例指示的所有东西都会解绑，所有事件监听会被移除，所有子实例也会被销毁。

##  父子、兄弟组件之间生命周期顺序

###  初始化/挂载

从父组件开始beforeCreate->created->beforeMount，进入子组件的生命周期beforeCreate->created->beforeMount，当所有子组件完成beforeMount，各个子组件才会到达mounted生命周期钩子，完成后到达父组件的。

###  更新

- 父子组件更新的数据**有**关联，顺序父beforeUpdate->子beforeUpdate->子updated->父updated
- 父子组件更新的数据**无**关联，顺序子beforeUpdate->子updated

###  销毁

同更新





#  Vue-Router

**介绍**：Vue Router是Vue官方的路由管理器，用来构建单页面应用。通过改变URL，在不请求页面的情况下，更新页面视图。

**功能**：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

##  前端路由原理

前端路由主要有两种实现方式，由mode参数决定使用何种方式：

```js
// 根据mode确定history实际的类并实例化    

// 根据mode确定history实际的类并实例化
switch (mode) {
  case 'history':
    this.history = new HTML5History(this, options.base)
    break
  case 'hash':
    this.history = new HashHistory(this, options.base, this.fallback)
    break
  case 'abstract':
    this.history = new AbstractHistory(this, options.base)
    break
  default:
    if (process.env.NODE_ENV !== 'production') {
      assert(false, `invalid mode: ${mode}`)
    }
}
```

- location.hash+hashchange事件
- history.pushState()+popstate事件

###  Hash模式

location.hash可读可写，在一般情况下为URL后“#”及其后面一部份组成，称为**锚点**。其特点为：

- hash虽然出现在URL中，但不会包括在HTTP请求中。作用为指导浏览器的动作，对服务器端完全无用，改变hash不会重新加载页面
- 在hash值发生变化时会触发onhashchange事件（**原理**）
- 每一次改变hash都会在浏览器的访问历史中增加一个记录

####  路由更新方式

#####  主动触发

router-link绑定了click方法，触发history.push或者history.replace，从而触发history.transitionTo转换路由，使用updateRoute更新`_route`，使用beforeCreate中劫持`_route`的方法，当其变化，触发router-view的变化

- HashHistory.push()

  将新路由添加到浏览器访问历史的栈顶

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190820100505561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI3Njc0NDM5,size_16,color_FFFFFF,t_70)

  流程：

  ```vue
  $router.push()  //调用方法
  HashHistory.push()  //根据hash模式调用，设置hash并添加到浏览器历史记录（栈顶）
  History.transitionTo()  //监测更新，更新则调用History.updateRoute()
  History.updateRoute()  //更新路由
  {app._route = route}  //替换当前app路由
  vm.render()  //更新视图
  ```

- HashHistory.replace()

  这种方式是直接替换掉当前的路由

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190820100702439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI3Njc0NDM5,size_16,color_FFFFFF,t_70)

##### 地址变化

在hash值发生变化时会触发hashchange事件，HashHistory捕获到变化后会对应执行push或replace方法，从而调用transitionTo
，剩下的就和上面主动触发一样。

####  History模式

#####  主动触发

- push

  与hash模式相似，使用的是history.pushState

- replace

  同上，使用的是history.replaceState

#####  地址变化

URL发生变化，触发popstate事件

##  对比

- pushState设置的新URL可以是与当前URL同源的任意URL；而hash只可修改#后面的部分，故只可设置与当前同文档的URL
- pushState通过stateObject可以添加任意类型的数据到记录中；而hash只可添加短字符串
- pushState可额外设置title属性供后续使用
- history模式则会将URL修改得就和正常请求后端的URL一样,如后端没有配置对应/user/id的路由处理，则会返回404错误



#  slot 插槽

定义：插槽（slot）其实就是一块HTML模板。

对于任何一个组件，从模板种类的角度来分，可分为非插槽模板和插槽模板两大类。**非插槽模板**指的是HTML模板，比如`<div>、<ul>`等，其显示与隐藏以及如何显示由自身控制；**插槽模板**是slot，它的显示与隐藏以及怎样显示由父组件控制，插槽显示的**位置**由**子组件**自身决定。

形式上如下方例子，`<child></child>`为子组件，在父组件中，包含在`<child></child>`标签内的内容就是`<slot>`的内容

父组件

```xml
<template>
    <div class="father">
        <h3>这里是父组件</h3>
        <child>
            <div class="tmpl">
              <span>菜单1</span>
              <span>菜单2</span>
              <span>菜单3</span>
              <span>菜单4</span>
              <span>菜单5</span>
              <span>菜单6</span>
            </div>
        </child>
    </div>
</template>
```

子组件

```xml
<template>
    <div class="child">
        <h3>这里是子组件</h3>
        <slot></slot>
    </div>
</template>
```

##  匿名插槽/单个插槽/默认插槽

单个插槽不用设置name属性，，可以放置在组件的任意位置，但每个组件只能有一个这类插槽。

##  具名插槽

当插槽加了name属性，就变成具名插槽。具名插槽可以在一个组件中出现n次。

父组件：

```xml
<template>
    <div class="father">
       <child>
           	<template v-slot:header>
   				<h1>Here might be a page title</h1>					</template>

  			<p>A paragraph for the main content.</p>
  			<p>And another one.</p>

             <template v-slot:footer>
                <p>Here's some contact info</p>
             </template>
        </child>
    </div>
</template>
```

子组件：

```xml
<template>
    <div class="child">
        <header>
        	<slot name="header"></slot>  //具名插槽
        </header>
        <main>
        	<slot></slot>  //匿名插槽
        </main>
        <footer>
        	<slot name="footer"></slot>  //具名插槽
        </footer>
    </div>
</template>
```

渲染结果：

```xml
<div class="father">
	<div class="child">
    	<header>
    		<h1>Here might be a page title</h1>
  		</header>
        <main>
            <p>A paragraph for the main content.</p>
            <p>And another one.</p>
        </main>
        <footer>
            <p>Here's some contact info</p>
        </footer>
    </div>
</div>
```

**注意：在Vue2.6.0版本以后新加了`v-slot`属性用来代替`slot`和`slot-scope`属性**

```xml
//Vue 2.6.0及之后版本
////  父组件
<template v-slot:header>
	<h1>Here might be a page title</h1>
</template>
////  子组件
<slot name="header"></slot>



//Vue2.6.0之前版本
////  父组件
<h1 slot="header"></h1>
////  子组件
<slot name="header"></slot>
```

##  作用域插槽

定义：可以理解为带数据的插槽，要求在slot上绑定数据。

因为父组件决定是否显示与显示样式，而子组件决定在什么地方显示。当作用域插槽绑定了一套数据，其中的内容是由子组件决定。

```xml
//父组件
<template>
	<div>
    	<current-user>
        	<template v-slot:default="slotProps">
            	{{slotProps.user.firstName}}
            </template>
        </current-user>
    </div>
</template>

//子组件<current-user></current-user>
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

注意：之前版本使用`slot-scoped`



#  keep-alive

##  概念

keep-alive是一个**抽象组件**，它自身不会被渲染成一个DOM元素，也不会出现在父组件链中。使用keep-alive包裹动态组件时，会**缓存**不活动的组件实例。这样可以**避免组件被重新渲染**，节约性能。

**适用场景**：商品或订单列表页点击商品跳转到详情页，返回后仍显示原有信息（**并不会记录页面滚动位置，需注意**）

##  使用方法

- 在动态组件中的应用

  ```vue
  <keep-alive :include="whiteList" :exclude="blackList" :max="amount">
  	<component :is="currentComponent"></component>
  </keep-alive>
  ```

- 在vue-router中的应用

  ```vue
  <keep-alive :include="whiteList" :exclude="blackList" :max="amount">
  	<router-view></router-view>
  </keep-alive>
  ```

  **include**定义缓存白名单，keep-alive会缓存命中的组件；**exclude**定义缓存黑名单，被命中的组件将不会被缓存；**max**定义缓存组件上限，超出上限使用LRU的策略置换缓存数据。

##  生命周期（activated、deactivated独有）

（未使用keep-alive组件，页面回退仍会渲染页面，触发created钩子）

- 初次进入时：created > mounted > activated；退出后触发deactivated；
- 再次进入时：直接触发activated；
- 再次离开时：直接触发deactivated。

事件挂载的方法等，只执行一次的放在mounted中；组件每次进去执行的方法放在activated中。



#  Webpack

##  为什么使用Webpack

现今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，前端社区涌现出了很多好的实践方法。这些改进确实大大的提高了我们的开发效率，但是利用它们开发的文件往往需要进行**额外的处理**才能让浏览器识别,而手动处理又是非常繁琐的，这就为Webpack类的工具的出现提供了需求。

##  什么是Webpack

Webpack可以看作是模块打包工具，具体工作是：分析项目结构，找到JavaScript模块以及其他的一些浏览器不能直接运行的拓展语言，并将其转换和打包为合适的格式供浏览器使用（相当于中间桥梁）。

![image-20200326154311379](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200326154311379.png)

以Vue举例：浏览器不认识Vue，而我们写的单文件组件中都有可能包含HTML、JS、CSS甚至是图片资源等，并且单文件组件之间还有这复杂的关系。因此，项目若想被浏览器识别，我们就用webpack将其打包为浏览器认识的文件。

##  Webpack工作方式

将项目当作一个整体，通过一个给定的主文件，webpack将从这个主文件开始找到项目中所有的依赖文件，并使用loaders处理它们，最后打包为一个或多个浏览器可识别的文件。



#  vue-cli

vue-cli是脚手架工具，其作用就是用配置好的模板迅速搭建起一个项目工程来，省去自己配置webpack配置文件的基本内容。

![img](https://upload-images.jianshu.io/upload_images/14033715-51de19588254db82.png?imageMogr2/auto-orient/strip|imageView2/2/w/441/format/webp)

vue-cli的目的不是用来帮助安装webpack，而是依赖webpack，并帮助开发者配置好一套可用的webpack，省去配置工作的时间，专心业务。





#  组件与组件化

组件就是将一段UI**样式**和其对应的**功能**作为独立的整体去看待。

|          |              组件              |                     模块                     |
| :------: | :----------------------------: | :------------------------------------------: |
|  英文名  |           Component            |                    Module                    |
| 核心意义 |              复用              |               分离职责（功能）               |
| 分离模式 |              纵向              |                     横向                     |
| 分离标准 | 可以按UI分，也可以按逻辑代码分 | 按不同功能进行划分，每一个功能设计成一个模块 |

##  组件的特点

- 高内聚：将相关的功能逻辑和静态资源组织在一起并封装。这样可以让组件的功能明确，提高可靠性。
- 可重用：只要需要并合适，组件就可以重复使用，并且无论放在哪里都具有一样的功能和样式。
- 可互换：使用组件可以很容易的实现对功能的更改。
- 可组合：基于组件的架构让组件组合成新组件更加容易。

##  组件化

定义：使用组件对项目整体开发的思想。

- 页面上的每个**独立的**可视/可交互区域视为一个组件；
- 每个组件对应一个工程目录，组件所需各种资源都在这个目录下**就近维护**（分治策略 ）；
- 由于组件具有独立性，因此组件与组件之间可以自由组合；
- 页面只不过是组件的容器，负责组合组件形成功能完整的界面；
- 当不需要或替换某个组件时，可以整个组件删除或替换。







#  组件间通信方式

##  前言

因为组件实例的作用域是我相互独立的，这就意味着不同的组件之间的数据无法相互引用。

![image](https://segmentfault.com/img/remote/1460000019208629)

根据组件之间的关系，可以将组件之间的通信方式可以分为4种：

- 父传子
- 子传父
- 兄弟间通信
- 跨级通信

## 父->子

可以直接通过父子链子组件可以使用 `this.$parent` 来访问父组件实例；而父组件可以使用 `this.$children` 来访问它的所有子组件实例。**<font color=red>（十分不提倡使用）</font>**

父组件通过`props`的方式向子组件传递信息。

```vue
//App.vue父组件
<template>
  <div id="app">
    <users v-bind:users="users"></users>//前者自定义名称便于子组件调用，后者要传递数据名
  </div>
</template>
<script>
import Users from "./components/Users"
export default {
  name: 'App',
  data(){
    return{
      users:["Henry","Bucky","Emily"]
    }
  },
  components:{
    "users":Users
  }
}
</script>
```

```vue
//users子组件
<template>
  <div class="hello">
    <ul>
      <li v-for="user in users">{{user}}</li>//遍历传递过来的值，然后呈现到页面
    </ul>
  </div>
</template>
<script>
export default {
  name: 'HelloWorld',
  props:{
    users:{           //这个就是父组件中子标签自定义名字
      type:Array,
      required:true
    }
  }
}
</script>
```

###  注意事项

1. 传递数据注意在传递属性前加`v-bind`，将数据放到**data**中引用；
2. 如果prop传到子组件中的数据是一个对象的话，要注意传递的是一个**对象引用**，虽然父子组件看似是分离的但最后都是在同一对象下；
3. 如果所传值能被子组件直接使用且在父组件中不会变化，将其放到**data**中使用；若传入的数据在父组件中会变化，放到**mounted**属性中监听其变化***（要求先考虑好数据要如何使用）***

##  子->父

在子组件用`$emit`自定义事件，在父组件中监听这个事件，并接受子组件传递过来的数据。

```vue
// 子组件
<template>
  <header>
    <h1 @click="changeTitle">{{title}}</h1>//绑定一个点击事件
  </header>
</template>
<script>
export default {
  name: 'app-header',
  data() {
    return {
      title:"Vue.js Demo"
    }
  },
  methods:{
    changeTitle() {
      this.$emit("titleChanged","子向父组件传值");//自定义事件  传递值“子向父组件传值”
    }
  }
}
</script>
```

```vue
// 父组件
<template>
  <div id="app">
    <app-header v-on:titleChanged="updateTitle" ></app-header>//与子组件titleChanged自定义事件保持一致
   // updateTitle($event)接受传递过来的文字
    <h2>{{title}}</h2>
  </div>
</template>
<script>
import Header from "./components/Header"
export default {
  name: 'App',
  data(){
    return{
      title:"传递的是一个值"
    }
  },
  methods:{
    updateTitle(e){   //声明这个函数
      this.title = e;
    }
  },
  components:{
   "app-header":Header,
  }
}
</script>
```

###  注意事项

如果父组件需要子组件的数据，通过在标签中定义ref属性

##  兄弟间通信

###  路由URL参数

将需跨页面传递的数据放到url后面，跳转后从url字符串直接获取想要的参数。（只适合传递比较小的数据）

###  Bus通信

原理就是使用一个空的Vue实例作为中央事件总线。在实际应用中，通常在组件外定义一个bus.js作为组件间通信的桥梁。（适用于比较小型且不需要vuex又需要兄弟组件通信的情况）

1. bus.js中添加如下

   ```js
     import Vue from 'vue'
     export default new Vue
   ```

2. 组件中调用bus.js通过自定义事件传递数据

   ```js
     import Bus from './bus.js' 
     export default { 
         methods: {
            bus () {
               Bus.$emit('msg', '我要传给兄弟组件们')
            }
         }
     }
   ```

3. 兄弟组件中监听事件接受数据

   ```JS
       import Bus from './bus.js'
       export default {
           mounted() {
              Bus.$on('msg', (e) => {
                console.log(e)
              })
            }
          }
   ```

其他两种写法：

1. 直接将Bus注入到Vue根对象中，在子组件中通过this.$root.Bus.$on(),this.$root.Bus.$emit()来调用

   ```vue
   import Vue from 'vue'
   const Bus = new Vue()
   
   var app= new Vue({
       el:'#app',
   　　 data:{
   　　　　Bus
       }　　
   
   })
   ```

2. 将bus挂载到vue.prototype上

### Vuex

Vuex实现了一个单向数据流，在全局拥有一个State存放数据，当组件要更改State中的数据时，必须通过Mutation进行，Mutation同时提供了订阅者模式供外部插件调用获取State数据的更新。而当所有异步操作(常见于调用后端接口异步获取更新数据)或批量的同步操作需要走Action，但Action也是无法直接修改State的，还是需要通过Mutation来修改State的数据。最后，根据State的变化，渲染到视图上。

![image](https://segmentfault.com/img/remote/1460000019208632)

##  跨级通信

###  Vuex

不做赘述

###  provide/inject

Vue2.2.0新增。

功能：允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次多深，并在其上下游关系成立的时间里始终生效。

用法：祖先组件中通过`provider`来提供变量，然后在子孙组件中通过`inject`来注入变量。

####  注意事项

`provider`和`inject`绑定是不可响应的。

若想实现数据响应式：

- 方法一：provide祖先组件的实例，然后在子孙组件中注入依赖，这样就可以在子孙组件中直接修改祖先组件的实例的属性，不过这种方法有个缺点就是这个实例上挂载很多没有必要的东西比如props，methods
- 方法二：使用2.6最新API Vue.observable 优化响应式 provide(推荐)

###  $attrs/$listeners

Vue2.4新增，用于仅传递数据而不需要处理的情况。

- `$attrs`：包含了父作用域中不被 prop 所识别 (且获取) 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件。通常配合 interitAttrs 选项一起使用。
- `$listeners`：包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件

`$attrs`与`$listeners` 是两个对象，`$attrs` 里存放的是父组件中绑定的非 Props 属性，`$listeners`里存放的是父组件中绑定的非原生事件。





#  单文件组件

在很多Vue项目中，我们会使用`Vue.component`来定义组件，这种方式在小规模的项目中运行的很好，但不适用于大型的复杂的项目。因为：

- 全局定义要求每个component的命名不能重复
- 字符串模板缺乏语法高亮，增加代码阅读难度
- 不支持CSS
- 缺少构建步骤，不能使用预处理器，如 Pug (formerly Jade) 和 Babel

为了使开发更为方便，增大组件的可复用性，单文件组件就产生了。

##  介绍

单文件组件指的是文件后缀名为`.vue`的文件，这种文件每一个都是一个组件，要想实现组件的复用只需要引入相应的`.vue`文件。

###  优点

- 代码集中，便于开发、管理和维护
- 可复用性高，直接将vue文件拷贝到项目中即可

###  简介

`.vue` 文件是一个自定义的文件类型，用类 HTML 语法描述一个 Vue 组件。每个 `.vue` 文件包含三种类型的顶级语言块 `<template>`、`<script>` 和`<style>` ，还允许添加可选的自定义块：

```vue
//非自定义的三个模块都可以通过src属性导入外部文件
<template>
  <div class="example">{{ msg }}</div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>

<style>
.example {
  color: red;
}
</style>

<custom1>
  This could be e.g. documentation for the component.
</custom1>
```

`vue-loader` 会解析文件，提取每个语言块，如有必要会通过其它 loader 处理，最后将他们组装成一个 ES Module，它的默认导出是一个 Vue.js 组件选项的对象。

##  语言块（具体实现）

###  模板`<template>`

- 每个`.vue`文件最多包含一个`<template>`块
- <font color=red>`<template>`中的内容被提取并传递给`vue-template-compiler`为字符串，预处理为JavaScript渲染函数，并最终注入到从`<script>`导出的组件中。</font>

###  脚本`<script>`

- 每个 `.vue` 文件最多包含一个 `<script>` 块。
- 这个脚本会作为一个 <font color=red>ES Module</font> 来执行。
- 它的**默认导出**应该是一个 Vue.js 的<font color=red>组件选项对象</font>。也可以导出由 `Vue.extend()` 创建的扩展对象，但是普通对象是更好的选择。
- 任何匹配 `.js` 文件 (或通过它的 `lang` 特性指定的扩展名) 的 webpack 规则都将会运用到这个 `<script>` 块的内容中。

###  样式`<style>`

- 默认匹配：`/\.css$/`。
- 一个 `.vue` 文件可以包含多个 `<style>` 标签。
- `<style>`标签可以有`scoped`或者`module`属性 (查看 scoped CSS和 CSS Modules) 以帮助你将样式封装到当前组件。具有不同封装模式的多个 `<style>`标签可以在同一个组件中混合使用。
- 任何匹配 `.css` 文件 (或通过它的 `lang` 特性指定的扩展名) 的 webpack 规则都将会运用到这个 `<style>` 块的内容中。





#  单页应用

##  概念

**单页应用**：指的是在一个页面上集成多种功能，甚至整个系统只有一个页面，所有的业务功能都是它的子模块，通过特定的方式挂接到主界面上。

![img](https://upload-images.jianshu.io/upload_images/3865021-ec2263b36a17c201.png?imageMogr2/auto-orient/strip|imageView2/2/w/702/format/webp)



**多页应用**：每一次页面跳转的时候，后台服务器都会给返回一个新的`html`文档，这种类型的网站也就是多页网站，也叫做多页应用。![img](https://upload-images.jianshu.io/upload_images/3865021-dd529dee8fe39dc4.png?imageMogr2/auto-orient/strip|imageView2/2/w/691/format/webp)

##  对比

| mm                         | 多页应用模式MPA                                          | 单页应用模式SPA                                              |
| -------------------------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| 应用构成                   | 由多个完整页面构成                                       | 一个外壳页面和多个页面片段构成                               |
| 跳转方式                   | 页面之间的跳转是从一个页面跳转到另一个页面               | 页面片段之间的跳转是把一个页面片段删除或隐藏，加载另一个页面片段并显示出来。这是片段之间的模拟跳转，并没有开壳页面 |
| 跳转后公共资源是否重新加载 | 是                                                       | 否                                                           |
| URL模式                    | `http://xxx/page1.html 和 http://xxx/page2.html`         | `http://xxx/shell.html#page1 和 http://xxx/shell.html#page2` |
| 用户体验                   | 页面间切换加载慢，不流畅，用户体验差，特别是在移动设备上 | 页面片段间的切换快，用户体验好，包括在移动设备上             |
| 能否实现转场动画           | 无法实现                                                 | 容易实现（手机`app`动效）                                    |
| 页面间传递数据             | 依赖`URL、cookie`或者`localstorage`，实现麻烦            | 因为在一个页面内，页面间传递数据很容易实现(这里是我补充，父子之间传值，或`vuex`或`storage`之类) |
| 搜索引擎优化（SEO）        | 可以直接做                                               | 需要单独方案做，有点麻烦                                     |
| 特别适用的范围             | 需要对搜索引擎友好的网站                                 | 对体验要求高的应用，特别是移动应用                           |
| 搜索引擎优化（SEO）        | 可以直接做                                               | 需要单独方案做，有点麻烦                                     |
| 开发难度                   | 低一些，框架选择容易                                     | 高一些，需要专门的框架来降低这种模式的开发难度               |

##  如何实现切换

**（简易说法）**JS会感知到Url的变化，通过这一点，可以用JS动态的将当前页面的内容清除掉，然后将下一个页面的内容挂载到当前页面上，这个时候的路由不是后端来做了，而是前端来做，判断页面到底是显示哪个组件，清除不需要的，显示需要的组件。这种过程就是单页应用，每次跳转的时候不需要再请求html文件了。





#  Vuex

`Vuex`是一个专为Vue.js应用程序开发的**状态管理模式**。它采用**集中式**存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

##  前置知识（简单状态管理）

在Vue中，当访问数据对象时，一个Vue实例只是简单的代理访问。若有某个或某些状态需要被多个实例共享，那么就可以通过维护一份数据来实现共享。于是设计了**store模式**：相当于是一个共享对象，所有 store 中 state 的改变，都放置在 store 自身的 action 中去管理。这种集中式状态管理能够被更容易地理解哪种类型的 mutation 将会发生，以及它们是如何被触发。当错误出现时，我们现在也会有一个 log 记录 bug 之前发生了什么。![状态管理](https://cn.vuejs.org/images/state.png)

```javascript
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
```

这个状态自管理应用包含以下几个部分：

- **state**，驱动应用的数据源；
- **view**，以声明方式将 **state** 映射到视图；
- **actions**，响应在 **view** 上的用户输入导致的状态变化。

以下是一个表示“单向数据流”理念的简单示意：![img](https://vuex.vuejs.org/flow.png)

但这种模式遇到多个组件共享状态时，此模式不再适用。

因此，设计者提取出组件的共享状态，以一个全局单例模式管理。

![vuex](https://vuex.vuejs.org/vuex.png)

##  核心概念

###  State

state可以理解为store中的共享数据

###  Getter

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

###  Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

mutation中的函数可以接受额外参数，被称为mutation的载荷（payload）

注意：mutation必须是同步函数

###  Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意**异步**操作。

让我们来注册一个简单的 action：

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。

Action 通过 `store.dispatch` 方法触发

 `store.dispatch` 可以处理被触发的 action 的处理函数返回的 Promise，并且 `store.dispatch` 仍旧返回 Promise。因此可以实现异步。

###  Module

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```js
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```



