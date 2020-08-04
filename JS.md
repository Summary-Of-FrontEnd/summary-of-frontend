# location的一些属性

**http://www.joymood.cn:8080/test.php?user=admin&pwd=admin#login**

1. location.href：得到整个如上的完整url
2. location.protocol：得到传输协议http:
3. location.host：得到主机名连同端口http://www.joymood.cn:8080/
4. location.hostname：得到主机名http://www.joymood.cn/
5. location.pathname：得到主机后部分不包括问号?后部分的/test.php
6. location.search：得到url中问号?之后井号#之前的部分?user=admin&pwd=admin
7. location.hash：得到#之前的部分#login
8. 





#  `==`与`===`

在JS中，判断是否相等相等一共有两种操作符`==`与`===`

`==`：相等与不相等——先转换类型再比较

`===`：全等与不全等——未经转换直接比较

##  `==`

`==`与`!=`这两个操作符都会先转换操作数（强制转型），然后再做比较

因此，在比较的数据类型不同是，会按照一定的规则进行**类型转换**，基本规则为：

- 如果有一个操作数是布尔值，则在比较相等性之前先将其转换为数值——`false`转换为0，`true`转换为1
- 如果一个操作数是字符串，另一个操作数是数值，则在比较相等性之前先将字符串转换为数值
- 如果一个操作数是对象，另一个操作数不是，则调用`valueOf()`方法，用得到的基本类型值按照前面的规则进行比较

类型转换完成后，按照以下规则进行**比较**：

- `null`和`undefined`是相等的
- 比较相等性之前，不能将`null`和`undefined`转换成其他任何值
- 如果有一个操作数是`NaN`，则相等操作符返回`false`，而不相等操作符返回`true`。重要提示：即使两个操作数都是`NaN`，相等操作符也返回`false`，因为按照规则，`NaN`不等于`NaN`
- 如果两个操作数都是对象，则比较他们是不是同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回`true`。否则，返回`false`；

##  `===`

**全等操作符**由3个等于号（===）表示，它只在两个操作数未经转换就相等的情况下返回true





#  null与undefined

null转为数值为0，undefined转为数值为NaN

null表示“没有对象”，即该处不应该有值。典型用法：

- 作为函数的参数，表示该函数的参数不是对象
- 作为对象原型链的终点

undefined表示“缺少值”，即此处应该有个值，但还未定义。典型用法：

- 变量被声明了，但没有赋值
- 调用函数时，应该提供的参数没有提供
- 对象没有赋值的属性
- 函数没有返回值





#  Symbol

##  前言

ES5的对象属性名都是字符串，容易造成属性名冲突（例如为对象添加新的属性时）。为了避免这样的事情发生，ES6引入了一种新的基础数据类型Symbol，功能类似于一种标识唯一性的ID，保证每个属性的名字都是独一无二的。

##  概念

因此Symbol作为对象的属性名而被创造，用于保证不会与其他属性名产生冲突。每个由Symbol()函数产生的Symbol值都是独一无二的。

```js
let s = Symbol();
typeof s  //"symbol"
```

**注**：Symbol是一个原始类型的值（**类字符串数据类型**），不是对象。即由于Symbol值不是对象，所以不能添加属性。

Symbol函数可以接受一个字符串作为参数，表示对Symbol实例的**描述**，主要是为了在控制台显示，或者转为字符串时比较容易区分。

```js
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"

```

Symbol值不能与其他类型的值进行运算，会报错。但可以显示转为字符串（Symbol.prototype.description）与布尔值。

当Symbol值作为对象属性时只能使用方括号[]，不能用点.

###  遍历

当Symbol作为属性名时，不能使用`for...in`、`for...of`、`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`遍历。

使用`Object.getOwnPropertySymbols()`方法获得指定对象的Symbol属性名。该方法返回一个数组，成员是当前对象的所有用作属性名的Symbol值。

还可以通过使用反射的`Reflect.ownKeys()`返回所有类型的键名（常规键名与Symbol键名）。

鉴于这一特性，可以为对象定义一些非私有但又希望只用于内部的方法。

##  其他

###  Symbol.for()  Symbol.keyFor()

Symbol.for()：用于生成相同的Symbol值

```js
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
```

`Symbol.keyFor()`方法返回一个已登记的 Symbol 类型值的`key`

```js
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // s2未登记，值为undefined
```

##  应用

1. 使用Symbol来作为对象的属性名（key）
2. 使用Symbol来代替常量
3. 使用Symbol定义类的私有属性/方法



#  判断数据类型

##  typeof

返回数据类型有：number、boolean、symbol、string、object、undefined、function

typeof null 返回类型错误，返回object







#  var、let、const

> 此为对var、let、const区别的简单总结，参考了[阮一峰老师的《ECMAScript入门》](https://es6.ruanyifeng.com/#docs/let)

##  前言

在ES2015之前，JS只有两种类型的作用域：全局作用域和函数（局部）作用域。

在全局作用域声明的变量叫全局变量,可以在JS程序中任何位置访问.

局部变量只能在他们被声明的函数内访问。

块级作用域是ES2015新增，每个{}就代表一个块级作用域，只对`let`和`const`有用。

##  var let

let不存在变量提升，即变量必须在声明后才能使用，否则报错。

> 注：var变量提升指的是
>
> - 所有的声明都会提升到作用域的最顶上去（只会提升声明，不会提升初始化）
> - 同一个变量只会声明一次，其他的会被忽略掉或者覆盖掉
> - 函数声明的优先级高于变量声明的优先级，并且函数声明和函数定义的部分一起被提升

暂时性死区：只要块级作用域内存在let命令，它所声明的变量就绑定在这个区域，不受外部影响。即在代码块内，使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”

let不允许在重复声明，var可以

##  const

const声明一个只读常量。一旦声明，常量的值就不能改变（要求一旦声明就必须立即初始化）。使用方法等和let几乎一样。

**本质**：并不是变量的值不得改动，而是变量指向的那个**内存地址所保存的数据**不得改动。因此，对于简单类型的数据（数值、字符串、布尔值），值保存在变量指向的那个内存地址，等同于常量。但对于复合类型的数据（数组、对象），保存的只是一个**指向实际数据的指针**，只会保证这个**指针固定**，并不能保证其指向的数据结构不变。



#  reduce()

`reduce()`方法（2个参数）对数组中的每个元素执行一个**reducer**函数（4个参数），将其**结果汇总**为单个返回值。

语法

```js
arr.reduce(callback(acc,cur,idx,src),initialValue)
```

参数

- callback：执行数组中每个值（若没提供初始值则第一个值除外）的函数，包含4个参数：
  - acc：accumulator，累计器累计回调的返回值；它是上一次回调时返回的累计值或初始值
  - cur：currentValue，数组中正在处理的元素
  - idx（可选）：index，数组中正在处理的当前元素的索引。若提供了初始值，则索引号为0，否则从1开始
  - src（可选）：source array，调用reduce()的数组
- initialValue（可选）：作为第一次调用回调函数时的第一个参数的值。若没有，则使用数组中第一个元素

##  应用场景

- 数组元素求和

- 将二维数组转化为一维

  ```js
  var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
    function(a, b) {
      return a.concat(b);
    },[]
  );
  // flattened is [0, 1, 2, 3, 4, 5]
  ```
  
- 计算数组中每个元素出现的次数

- 按属性对Object分类

- 数组去重

  ```js
  let arr = [1,2,1,2,3,5,4,5,3,4,4,4,4];
  let result = arr.sort().reduce((init, current) => {
      if(init.length === 0 || init[init.length-1] !== current) {
          init.push(current);
      }
      return init;
  }, []);
  console.log(result); //[1,2,3,4,5]
  ```

- 按顺序运行Promise

- ……………………





#  箭头函数

箭头函数是ES6新增的一种函数形式，其形式时用一个箭头=>代替function，所以叫作箭头函数。相当于匿名函数，并且简化了函数定义。

有两种简写规则：

- 当参数只有一个时；
- 函数内部只有一条return语句时

作用：

- 更简短的函数且不绑定`this`

  箭头函数不会创建自己的`this`，只会从自己的作用域链的上一层继承`this`。因此，通过`call()`或`apply()`方法调用时，只能传递参数，第一个参数会被忽略。

- 不绑定`arguments`

  箭头函数不绑定Arguments对象

注意：

- 不能作为对象的属性
- 不能作为构造函数
- 不能作为原型方法





#  闭包

##  定义

在MDN上的定义为：函数和对其周围环境的引用捆绑在一起构成闭包，可以让使用者从函数内部访问外部函数的作用域。

- 定义中的周围环境指的是**词法环境**或者叫**词法作用域**。作用为根据源代码中声明变量的位置来确定该变量在何处可用。

**词法作用域：即静态作用域，就是指在书写代码时函数声明/定义的位置来决定**

动态作用域：以调用为准

因此，闭包可以通俗的理解为：**函数**与函数**内部能访问到的变量**的总和。

形如

```js
function funA(){
    var a = 10;
    return function(){
        console.log(a)
    }
}

var b = funA();
b();
```

##  作用

1. 可以在函数的外部访问到函数内部的局部变量
2. 可以让这些变量始终保存在内存中，不会随着函数的结束而自动销毁

##  闭包为什么存在

**闭包存在的意义就是其作用。**

首先，JS和其他语言一样，拥有作用域，其最大的作用就是**隔离变量**。并且作用域存在上下级关系，由函数是在哪个作用域下创建所确定。

变量取值的过程是先在当前作用域查找值，如果没有则会跳转到上级作用域查找，如果找到就停止，未查找到这继续查找上级作用域直到全局作用域中。

```js
var a = 10;
function fn(){
    var b = 20;
    function bar(){
        console.log(a + b)
    }
    return bar
}

var x = fn(),
    b = 200;
x() //值为10+20=30
```

变量取值的过程就像一个**单向链表**，只能从当前作用域开始向上级作用域查找，因此我们也将这个查找过程称为作用域链。

从上面可以看到，函数内部可以访问到外部的变量，而外部无法访问到内部作用域的变量。所以在某些情况下，我们不想某个变量直接暴露，就将这个变量保存在函数中，使其变为局部变量，再使用闭包将其“**间接暴露**”。

```js
(function countNum(){
    var num = 0;
    window.addNum = function (){
        num++;
        console.log(num)
    }
    window.reduceNum = function (){
        num--;
        console.log(num)
    }
}())
```

在这个例子中，要想改变num的值，只能通过`addNum`和`reduceNum`两个函数。







#  继承

定义：可以使得子类别**具有父类别的各种属性和方法**，而不需要编写相同的代码。在令子类别继承父类别的同时，可以重新定义某些属性，并重写某些方法，即**覆盖父类别的原有属性和方法**，使其获得与父类别不同的功能。同时也能**添加新的属性和方法**。



##  实现方式

### 一、属性拷贝

定义：将对象的所有成员复制一份给需要继承的对象。

存在问题：因为是**浅拷贝**，对于是引用类型的成员继承过来后，对其进行修改会对父对象产生影响。

```js
// 创建父对象
var superObj = {
  name: 'Li',
  age: 25,
  friends: ['小明', '小李', '小赵'],
  showName: function(){
    alert(this.name);
  }
}

// 创建需要继承的子对象
var subObj = {};

// 开始拷贝属性(使用for...in...循环)
for( var i in superObj ){
  subObj[i] = superObj[i];
}

console.log(subObj)
console.log(superObj)
```

### 二、原型式继承

定义：借用构造函数的原型对象实现继承（X.prototype）

存在问题：父构造函数的原型对象和子构造函数的原型对象上的成员有共享问题；只能继承父构造函数的原型对象上的成员，**不能继承继承父构造函数的实例对象的成员**。

```js
// 创建父构造函数
function SuperClass(name){
  this.name = name;
  this.showName = function(){
    alert(this.name);
  }
}

// 设置父构造器的原型对象
SuperClass.prototype.showAge = function(){
  console.log(this.age);
}

// 创建子构造函数
function SubClass(){

}

// 设置子构造函数的原型对象实现继承
SubClass.prototype = SuperClass.prototype;//父原型对象改变，子原型对象跟着改变
//这种方法，只让子类获取了父构造函数的公有属性(prototype)，但不能获得其私有属性(this)

var child = new SubClass()
```

### 三、原型链继承

定义：即子构造函数.prototype = new 父构造函数()

存在问题：不能给父构造函数传递参数，父子结构函数的原型对象之间有共享问题

```js
// 创建父构造函数
function SuperClass(){
    this.name = 'liyajie';
    this.age = 25;
    this.showName = function(){
        console.log(this.name);
    }
}
// 设置父构造函数的原型
SuperClass.prototype.friends = ['小名', '小强'];
SuperClass.prototype.showAge = function(){
    console.log(this.age);
}
// 创建子构造函数
function SubClass(){

}
// 实现继承
SubClass.prototype = new SuperClass();
// 修改子构造函数的原型的构造器属性
SubClass.prototype.constructor = SubClass;

var child = new SubClass();
console.log(child.name); // liyajie
console.log(child.age);// 25
child.showName();// liyajie
child.showAge();// 25
console.log(child.friends); // ['小名','小强']

// 当我们改变friends的时候, 父构造函数的原型对象的也会变化
child.friends.push('小王八');
console.log(child.friends);["小名", "小强", "小王八"]
var father = new SuperClass();
console.log(father.friends);["小名", "小强", "小王八"]
```

### 四、借用构造函数

定义：使用**call和apply**借用其他构造函数的成员

存在问题：可以解决给父构造函数传递参数的问题, 但是获取不到父构造函数原型上的成员,也不存在共享问题

```js
// 创建父构造函数
function SuperClass(name){
  this.name = name;
  this.freinds = ['小王', '小强'];
  this.showName = function(){
     console.log(this.name);
  }
}

// 创建子构造函数
 function SubClass(name){
  // 使用call借用Person的构造函数
  SuperClass.call(this, name);
 }

 // 测试是否有了 Person 的成员
 var stu = new SubClass('Li');
 stu.showName(); // Li
 console.log(stu.friends); // ['小王','小强']
```

### 五、组合继承

定义：借用构造函数+原型式继承

特点：父构造函数的属性继承到了子构造函数的实例对象上了,并且继承了父构造函数原型对象上的成员，解决了给父构造函数传递参数问题。

```js
// 创建父构造函数
function SuperClass(name,age){
    this.name = name;
    this.age = age;
    this.showName = function(){
        console.log(this.name);
    }
}
// 设置父构造函数的原型对象
SuperClass.prototype.showAge = function(){
    console.log(this.age);
}
// 创建子构造函数，将父构造函数的参数传递到子构造函数
function SubClass(name){
    SuperClass.call(this,name);//第二次调用
}
// 设置继承
SubClass.prototype = new SuperClass();//第一次调用
SubClass.prototype.constructor = SubClass;
```

### 六、借用构造函数+深拷贝

```js
function SuperClass(name,age){
    this.name = name;
    this.age = age;
    this.showName = function(){
        console.log(this.name);
    }
}
SuperClass.prototype.friends = ['小王','小强','小王八'];

function SubClass(name,25){
    // 借用构造函数(Person)
    SuperClass.call(this.name,25);
}
// 使用深拷贝实现继承
deepCopy(SubClass.prototype,SuperClass.prototype);
SubClass.prototype.constructor = SubClass;
```

###  七、寄生组合式继承（常用）

定义：通过借用构造函数来继承“私有”属性，使用原型链的方式来继承“公有”原型上的属性和方法。其原理就是只需要获得父类原型的一个副本，而不需要调用两次父类的构造函数。

```js
//获得原型副本
function object(o){
    function F(){};
    F.prototype = o;
    return new F()
}
//使子类获得父类原型上的属性和方法
funtion inheritPrototype(SubType,SuperType){
    let prototype = object(SuperType.prototype);
    prototype.constructor = SubType;
    Subtype.prototype = prototype
}

function SuperClass(name,age){
    this.name = name;
    this.age = age;
    this.showName = function(){
        console.log(this.name);
    }
}
SuperClass.prototype.sayAge = function(){
    console.log(this.age);
}
//使子类获得父类构造函数实例属性
function SubClass(name,age){
    SuperClass.call(this,name);
    this.age = age
}

inheritPrototype(SubClass,SuperClass);

let sub = new SubClass("he",26)
```

总结下来就是：

![image-20200429184309281](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200429184309281.png)

- 子类构造函数的 **proto**指向父类构造器，继承父类的静态方法
- 子类构造函数的 prototype的 **proto**指向父类构造器的 prototype，继承父类的方法。
- 子类构造器里调用父类构造器，继承父类的属性。



> 以上是ES6出现前，实现继承的方式

ES5的继承实质是先创造子类的实例对象this，然后将父类的方法添加到this上面（Parent.apply(this)）。

ES6继承的实质是先将父类实例对象的属性和方法加到this上，然后再用子类的构造函数修改this（要求子类的构造函数必须执行一次super函数）。

### 八、ES6  class+extends

Class可以通过`extends`关键字实现继承

```js
//父类
class Point {
    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return this.x + this.y
    }
}
//子类
class ColorPoint extends Point {
    constructor(x,y,color) {
        super(x,y); //调用父类的constructor(x,y)
        this.color = color
    }
    
    toString() {
        return this.color + ' ' + super.toString 
    }
}
```

`super`关键字表示父类的构造函数，用来新建父类的this对象。子类必须在constructor方法中调用super方法，否则新建实例会报错（因为子类自己的this对象必须先通过父类的构造函数完成塑造）。

**注意**：在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错。

####  super关键字

super关键字既可以当作函数使用，也可以当作对象使用。

- super作为函数调用时，代表父类的构造函数
- super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类

####  prototype和`__proto__`

1）子类的`__proto__`属性，表示构造函数的继承，总是指向父类。

2）子类`prototype`属性的`__proto__`属性，表示方法的继承，总是指向父类的`prototype`属性。

```javascript
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```

####  Mixin模式

…………………………











#  深拷贝与浅拷贝

##  前言

###  JS数据类型

- 基本数据类型：String、Boolean、Number、Undefined、Null
- 引用数据类型：Object（Array、Date、RegExp、Function）

####  区别

保存位置不同：基本数据类型，名字和值（键值）都存储在栈内存中；引用数据类型，名字存在栈内存中，值存在堆内存中，但栈内存会提供一个引用的地址指向堆内存的值。

原因：

- 堆比栈大，栈比堆速度快；
- 基本数据类型稳定，相对来说占用的内存小；
- 引用类型数据大小是动态且无限的，所以将值存在堆中；
- 堆内存是无序存储，可以根据引用直接获取



因此，深拷贝与浅拷贝只针对于引用数据类型来说的。

##  定义

浅拷贝：只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。

深拷贝：会另外创造一个一摸一样的对象，新对象和原对象不共享内存，修改新对象不会改变原对象。

![img](https://upload-images.jianshu.io/upload_images/13263206-c651dc07788bf561.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

**区别**：浅拷贝只复制对象的第一层属性、深拷贝可以对对象的属性进行递归复制

###  浅拷贝的实现

1. for···in只循环第一层

   ```js
   // 只复制第一层的浅拷贝
   function simpleCopy(obj1) {
      var obj2 = Array.isArray(obj1) ? [] : {};
      for (let i in obj1) {
      obj2[i] = obj1[i];
     }
      return obj2;
   }
   var obj1 = {
      a: 1,
      b: 2,
      c: {
            d: 3
         }
   }
   var obj2 = simpleCopy(obj1);
   obj2.a = 3;
   obj2.c.d = 4;
   alert(obj1.a); // 1
   alert(obj2.a); // 3
   alert(obj1.c.d); // 4
   alert(obj2.c.d); // 4
   ```

2. Object.assign()

   ```js
   var obj = {
       a: 1,
       b: 2
   }
   var obj1 = Object.assign({},obj);
   boj1.a = 3;
   console.log(obj.a) // 3
   ```

3. 直接赋值

   ```js
   let a=[0,1,2,3,4],
       b=a;
   console.log(a===b);
   a[0]=1;
   console.log(a,b);
   ```

###  深拷贝的实现

####  采用递归去拷贝所有层级属性

```js
function deepClone(obj){
    let objClone = Array.isArray(obj)?[]:{};
    if(obj && typeof obj==="object"){
        for(key in obj){
            if(obj.hasOwnProperty(key)){
                //判断ojb子元素是否为对象，如果是，递归复制
                if(obj[key]&&typeof obj[key] ==="object"){
                    objClone[key] = deepClone(obj[key]);
                }else{
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}    
let a=[1,2,3,4],
    b=deepClone(a);
a[0]=2;
console.log(a,b);
```

缺陷：当遇到两个互相引用的对象，会出现死循环，为了避免相互引用的对象导致死循环，应该在遍历的时候判断是否相互引用对象，如果是则退出循环

####  通过JSON对象来实现拷贝（无法实现对对象中方法的深拷贝，会显示为undefined）

```js
function deepClone2(obj) {
  var _obj = JSON.stringify(obj),
    objClone = JSON.parse(_obj);
  return objClone;
}
```

####  通过jQuery的extend方法实现深拷贝

```js
var array = [1,2,3,4];
var newArray = $.extend(true,[],array); // true为深拷贝，false为浅拷贝
```

####  lodash函数库实现深拷贝

```js
let result = _.cloneDeep(test)
```

####  Reflect法

```js
// 代理法
function deepClone(obj) {
    if (!isObject(obj)) {
        throw new Error('obj 不是一个对象！')
    }

    let isArray = Array.isArray(obj)
    let cloneObj = isArray ? [...obj] : { ...obj }
    Reflect.ownKeys(cloneObj).forEach(key => {
        cloneObj[key] = isObject(obj[key]) ? deepClone(obj[key]) : obj[key]
    })

    return cloneObj
}
```





#  数组拍平

1.数组拍平也称数组扁平化，就是将数组里面的数组打开，最后合并为一个数组

2.实现

```
var arr = [1,2,[3,4,5,[6,7,8],9],10,[11,12]];
```

a：递归实现

```
function fn(arr){　　　　let arr1 = []
     arr.forEach((val)=>{
         if(val instanceof Array){
             arr1 = arr1.concat(fn(val))
         }else{
             arr1.push(val)
         }
      })
      return arr1
 }
```

b：reduce实现

```
function fn(arr){
    return arr.reduce((prev,cur)=>{
        return prev.concat(Array.isArray(cur)?fn(cur):cur)
    },[])
}
```





#  面向对象和面向过程

对象：具备某些特质或者能执行某些事的东西都可以称为对象。

**面向对象**：就是将构成问题的事物分解为各个对象，描述的是事物在解决问题的步骤中的行为。

- **优点**：易维护、易复用、易扩展，可以设计出低耦合的系统，使系统更加灵活、更易于维护
- **缺点**：性能比面向过程低

**面向过程**：就是分析出解决问题所需要的步骤，用函数将这些步骤一一实现，使用时再依次调用。

- **优点**：性能比面向对象高
- **缺点**：不易维护、复用和扩展



#  this关键词

`this`关键词指的是它所属的**对象**(在方法中就指向这个方法)。其拥有不同的值，具体取决于它的使用位置，也就是说this的指向在函数定义时无法确定，只有在函数执行时才能确定this的指向，因此可以理解为**this的最终指向为那个调用它的对象**。

- 情况1：如果一个函数中有this，但未被上一级对象调用，那么this指向window
- 情况2：如果一个函数中有this，这个函数有被上一级对象调用，那么this指向上一级对象
- 情况3：如果一个函数中有this，这个函数包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它的上一级对象

```js
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn; //只创建，并为执行/调用
j();  //此时才调用，所以this为window
```

**总结一下**：

- 在方法中，`this` 指的是**所有者对象**
- 单独的情况下，`this` 指的是**全局对象**
- 在函数中，`this` 指的是**全局对象**
- 在函数中，严格模式下，`this` 是 `undefined`
- 在事件中，`this` 指的是**接收事件的元素**
- 类似 call() 和 apply() 方法可以将 this 引用到**任何对象**

当前执行代码的环境对象，在非严格模式下，总是指向一个对象，在严格模式下可以是任意值

##  全局环境

无论是否在严格模式下，在全局执行环境中（在任何函数体外部）`this` 都指向全局对象。

浏览器中在全局环境下，使用`var`声明变量其实就是赋值给`this`或`window`。任何情况下，创建变量时没有使用`var`或者`let`(ECMAScript 6)，也是在操作全局`this`。

##  函数调用

###  正常调用

除了DOM的事件回调或者提供了执行上下文（后面会提到）的情况，函数正常被调用（不带`new`）时，里面的`this`指向的是**全局作用域**。

###  构造函数

当用调用函数时使用了`new`关键字，此刻`this`指代一个新的上下文，不再指向全局`this`，而是指向通过构造函数创建的这个实例

###  箭头函数

在箭头函数中，`this`与封闭词法作用域的`this`保持一致，也就是说，箭头函数的`this`在创建时就已决定，由其环境上下文决定。

箭头函数`this`的指向只能是全局或函数

注意：如果将`this`传递给`call`、`bind`、或者`apply`来调用箭头函数，它将被忽略。不过你仍然可以为调用添加参数，不过第一个参数（`thisArg`）应该设置为`null`。

##  对象中的this

`this` 指的是**所有者对象**





注：将函数赋给另一个变量，this会改变



#  call、apply、bind

call、apply、bind的作用相同，都是**改变函数运行时的this指向**（this的指向问题可参考——[this关键词](https://segmentfault.com/a/1190000022486405)）。

##  call

`call()`方法通过指定的`this`值和单独给出的**一个或多个**参数来调用一个函数。

> function.call(thisArg, arg1, arg2, ...)

- `thisArg`：可选，表示`function`函数调用时的`this`的值。值为`null`或`undefined`时，`this`指向全局对象。
- `arg1, arg2, ...`：参数列表。

实例代码：

```js
//应用一：继承
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

function Toy(name, price) {
  Product.call(this, name, price);
  this.category = 'toy';
}

var cheese = new Food('feta', 5);
var fun = new Toy('robot', 40);
```

```js
//应用二：调用匿名函数
var animals = [
  { species: 'Lion', name: 'King' },
  { species: 'Whale', name: 'Fail' }
];

for (var i = 0; i < animals.length; i++) {
  (function(i) {
    this.print = function() {
      console.log('#' + i + ' ' + this.species
                  + ': ' + this.name);
    }
    this.print();
  }).call(animals[i], i);
}
```

##  apply

`apply()`方法调用一个具有给定`this`值的函数，以及作为一个数组或类数组提供的参数。

> function.apply(thisArg, [argsArray])
>

- `thisArg`：必选，表示`function`函数调用时的`this`的值。值为`null`或`undefined`时，`this`指向全局对象。
- `argsArray`：可选，一个数组或类数组对象，其中的元素作为单独参数传给`function`函数。如果值为`null`或`undefined`，则表示不需要传入任何参数。

实例代码：

```js
//应用一：继承
function Person(name,age){   //定义一个类，人类  
   this.name=name;     //名字  
   this.age=age;       //年龄 
   this.sayhello=function(){alert("hello")};
} 
function Print(){            //显示类的属性 
   this.funcName="Print"; 
   this.show=function(){      
       var msg=[];
       for(var key in this){ 
           if(typeof(this[key])!="function"){
               msg.push([key,":",this[key]].join(""));
           }
       } 
       alert(msg.join(" "));
   };
} 
function Student(name,age,grade,school){    //学生类 
   Person.apply(this,arguments);
   this.grade=grade;                //年级 
   this.school=school;                 //学校 
} 
var p1=new Person("jake",10);
p1.sayhello();
var s1=new Student("tom",13,6,"清华小学");
s1.show();
s1.sayhello();
alert(s1.funcName);
```

```js
//应用二：令数组能够使用Number对象的方法
function getMax2(arr){
    return Math.max.apply(null,arr);
}
```

##  bind

`bind()`方法创建一个新的函数，在`bind()`被调用时，这个新函数的`this`被指定为`bind()`的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

> function.bind(thisArg, arg1, arg2, ...)

- `thisArg`：调用绑定函数时作为`this`参数传递给目标的函数的值。
- `arg1, arg2, ...`：当目标函数被调用时，被预置入绑定函数的参数列表中的参数。

##  比较

|            |             call             |            apply             |                  bind                  |
| :--------: | :--------------------------: | :--------------------------: | :------------------------------------: |
|   <font color=red>作用</font>     |  改变函数运行时的`this`指向  |  改变函数运行时的`this`指向  |       改变函数运行时的`this`指向       |
| 第一个参数 | `this`，代表调用此函数的对象 | `this`，代表调用此函数的对象 |      `this`，代表绑定此函数的对象      |
|  剩余参数  |        一个或多个参数        |       数组或类数组对象       |             一个或多个参数             |
| 使用结果 | 直接调用函数 | 直接调用函数 | 相当于声明一个新函数 |
|   返回值   |   返回调用该函数所返回的值   |   返回调用该函数所返回的值   | 返回一个改变了上下文`this`后的**函数** |

##  手写实现

###  call

```js
Function.prototype.myCall = function(context) {
    //判断传入要绑定的对象，值为null或undefined时就默认是window
    if(context===null || context===undefined) {
        context = window
    }
    //在指向的对象上新建一个fn属性，值为this，也就是fn()
    context.fn = this;
    //保存返回值
    let result;
    //对传入的数据进行操作
    let args = [...arguments].slice(1);
    //执行方法并保存返回值
    result = context.fn(args);
    //删除该属性
    delete context；
    //返回结果
    return result
}


let obj = {
    value: '123'
}

function fn(name,age) {
    return {
        value: this.value,
      name,
      age
    }
}

fn.myCall(obj, 'LJ', 25); // {value: "hdove", name: "LJ", age: 25}
```

###  apply

```js
//代码几乎相同，只有对传入参数的操作有区别
Function.prototype.myApply = function(context, args) {
  var context = Object(context) || window;
  context.fn = this;
  let result;
  //对传入的数据进行操作
  let args = [...arguments].slice(1);  
  //判断有没有传入args
  if(!args) {
    result = context.fn();
  }else {
    result = context.fn(...args);
  }
  delete context.fn;
  return result;
}


const obj = {
  value: 'hdove'
}

function fn(name, age) {
  return  {
      value: this.value,
      name,
      age
  }
}

fn.myApply(obj, ['LJ', 25]); // {value: "hdove", name: "LJ", age: 25}
```

###  bind

```js
Function.prototype.myBind = function(context) {
    if (typeof this !== "function") {
        throw new Error("不是一个函数");
    }
    const self = this;
    // 第一个参数是this，截取掉
    const args1 = [...arguments].slice(1); 
    return function() {
        // 获取调用时传入的参数
        const args2 = [...arguments];
        return self.apply(context, args1.concat(args2));
    }
}

const obj = {
  value: 'hdove'
}

function fn(name, age) {
    console.log(this.value);
    console.log(name);
    console.log(age);
}

var bindFn = fn.myBind(obj, 'LJ');

bindFn(25); // 'hdove' 'LJ' 25
```



#  数据类型的判断

##  typeof操作符

typeof是一个运算符，有2种使用方式：`typeof(表达式)`、`typeof 变量名`。利用typeof除了Array和null判断为object外，其他类型都能正常判断。

##  instanceof操作符

**`instanceof`** **运算符**用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

##  constructor属性

与instanceof类似，根据对象的构造函数判断类型

##  其他

- **使用 Object.prototype.toString 来判断**
- **使用 原型链 来完成判断**
- **Array.isArray()**







#  Mock

**概念**：Mock.js是一个模拟数据生成器，可以用来**拦截Ajax请求**，直接返回模拟数据。

**应用场景**：当后端还未完成数据输出或者接口没写好的情况下，为了不保证开发的正常进行，前端只好自己写静态模拟数据。

**优点**：

1. 前后端分离：让前端工程师独立于后端进行开发
2. 增加单元测试的真实性：通过随机数据，模拟各种场景
3. 开发无侵入：不需要修改既有代码，就可以拦截Ajax请求，返回模拟的响应数据
4. 用法简单：符合直觉的接口
5. 数据类型丰富：支持生成随机的文本、数字、布尔值、日期、邮箱、链接、图片、颜色等。
6. 方便支持：支持支持扩展更多数据类型，支持自定义函数和正则。



#  函数节流

**概念**：限制**一个函数**在**一定时间**内只能**执行一次**

**目的**：最主要的为了节约计算机资源，还为了优化程序性能，提高用户体验。

**适用场景**：

1. 懒加载、滚动加载、加载更多或监听滚动条位置；
2. 百度搜索框，搜索联想功能；
3. 防止高频点击提交，防止表单重复提交；

##  为什么需要函数节流

因为在前端开发过程中，有一些事件或函数会频繁地触发（例如onresize,scroll,mousemove,mousehover等），如果不做限制不仅会造成计算机资源的浪费，还会降低程序运行速度，甚至造成浏览器卡死、崩溃。并且重复的 ajax 调用不仅可能会造成请求数据的混乱，还会造成网络拥塞，占用服务器带宽，增加服务器压力。

##  解决思路

主要实现思路就是通过**`setTimeout`**定时器，通过设置延时时间，在第一次调用时，创建定时器，先设定一个变量true，写入需要执行的函数。第二次执行这个函数时，会判断变量是否是true，是则返回。当第一次的定时器执行完函数最后会设定变量为false。那么下次判断变量时则为false，函数会依次运行。目的在于一定时间内，保证多次函数的请求只执行最后一次调用。

##  代码实现

**方法一：时间戳方案**

```js
// 时间戳方案
function throttle(fn,wait){
    var pre = Date.now();
    return function(){
        var context = this;
        var args = arguments;
        var now = Date.now();
        if( now - pre >= wait){
            fn.apply(context,args);
            pre = Date.now();
        }
    }
}

function handle(){
    console.log(Math.random());
}
    
window.addEventListener("mousemove",throttle(handle,1000));
//throttle会返回一个匿名函数，每当触发事件时就会调用这个匿名函数
```

**方法二：定时器方案**

```js
// 定时器方案
function throttle(fn,wait){
    var timer = true;
    return function(){
        var context = this;
        var args = arguments;
        if(!timer) return;
        timer = false;
        setTimeout(() => {
            fn.apply(context,arguments);
            timer = true
        },wait);
    }
}
    
function handle(){
    console.log(Math.random());
}
    
window.addEventListener("mousemove",throttle(handle,1000));
//throttle会返回一个匿名函数，每当触发事件时就会调用这个匿名函数
```





#  函数防抖

**概念**：指触发事件后，在n秒内函数只能执行一次，如果触发事件后在n秒内又触发了事件，则会重新计算函数延时执行时间。即**一定时间内多次**触发同一事件，只执行**最后一次**，或者只执行**最开始的一次**，中间的不执行。

**适用场景**：

1. 搜索框搜索输入。只需用户最后一次输入完，再发送请求；
2. 用户名、手机号、邮箱输入验证；
3. 浏览器窗口大小改变后，只需窗口调整完后，再执行 **resize** 事件中的代码，防止重复渲染。

##  为什么需要函数防抖

因为在前端开发过程中，有一些事件或函数会频繁地触发（例如onresize,scroll,mousemove,mousehover等），如果不做限制不仅会造成计算机资源的浪费，还会降低程序运行速度，甚至造成浏览器卡死、崩溃。并且重复的 ajax 调用不仅可能会造成请求数据的混乱，还会造成网络拥塞，占用服务器带宽，增加服务器压力。

##  解决思路

###  非立即执行

意思是触发事件后函数不会立即执行，而是在n秒后执行，如果在n秒内又出发了事件，则会重新计算函数执行时间。

```js
//非立即执行
function debounce(func,wait) {
    let timer;
    return function() {
        var context = this;
        var args = arguments;
        if(timer) clearTimeout(timer);
        timer = setTimeout(() => {
            func.apply(context,args)
        },wait)
    }
}
function handle(){
    console.log(Math.random());
}
    
window.addEventListener("mousemove",debounce(handle,1000));
//debounce会返回一个匿名函数，每当触发事件时就会调用这个匿名函数。在这个匿名函数中的延时函数运行完之前再触发事件会重新调用此匿名函数，此时的timer为setTimeout函数；当匿名函数完全运行完，timer则被释放。
```

###  立即执行

意思是触发事件后函数会立即执行，然后n秒内不触发事件才能继续执行函数的效果。

```js
//立即执行
function debounce(func,wait) {
    let timer;
    return function () {
        let context = this;
        let args = arguments;
        if (timer) clearTimeout(timer);
        let callNow = !timer;
        timer = setTimeout(() => {
            timer = null;
        }, wait)
        if (callNow) func.apply(context, args)
    }
}

function handle(){
    console.log(Math.random());
}

 window.addEventListener("mousemove",debounce(handle,1000); 
```



#  Proxy

概念：在目标对象之前假设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，经过这一层我们可以统一处理，因此将它叫作“代理(Proxy)器”。

语法`const p = new Proxy(target,handler)`。其中`target`为要使用Proxy包装的目标对象；`handler`为一个通常以函数作为属性的对象，各属性中的函数分别定义了在执行各种操作时代理`p`的行为。

##  handler对象

`handler`对象是一个容纳一批特定属性的占位符对象，包含`Proxy`的各个捕获器。

###  handler.get()

**功能**：handler.get()方法用于拦截对象的读取属性操作。

**语法**:

```js
var p = new Proxy(target, {
    get: function(target,property,receiver) {
        //...
    }
})
```

**参数**（this上下文绑定在handler对象上）：

- target：目标对象
- property：被获取的属性名
- receiver：Proxy或继承Proxy的对象

###  handler.set()

**功能**：handler.set()方法是设置属性值操作的捕获器。

**语法**:

```js
var p = new Proxy(target, {
    set: function(target,property,value,receiver) {
        //...
    }
})
```

**参数**（this绑定在handler对象上）：

- target：目标对象
- property：被获取的属性名或symbol
- value：**新**属性值
- receiver：最初被调用的对象，通常是proxy本身

**返回值**：set()方法应当返回一个布尔值（true代表属性设置成功）。

###  handler.apply()

**功能**：handler.apply()方法用于拦截函数的调用。

**语法**:

```js
var p = new Proxy(target, {
    apply: function(target,thisArg,argumentsList) {
        //...
    }
})
```

**参数**（this绑定在handler对象上）：

- target：目标对象（函数）
- thisArg：被调用时的上下文对象
- argumentsList：被调用时的参数数组

**返回值**：返回任何值。

### handler.construct()

**功能**：handler.construct()方法用于拦截`new`操作符。

**语法**:

```js

var p = new Proxy(target, {
    construct: function(target,argumentsList,newTarget) {
        //...
    }
})
```

**参数**（会传递给`construct`方法，this绑定在handler对象上）：

- target：目标对象
- argumentsList：被调用时的参数数组
- newTarget：最初被调用的构造函数

**返回值**：必须返回一个对象。

###  handler.has()

**功能**：handler.has()方法是针对`in`操作符的代理方法。

**语法**:

```js
var p = new Proxy(target, {
    has: function(target,prop) {
        //...
    }
})
```

**参数**（会传递给`construct`方法，this绑定在handler对象上）：

- target：目标对象
- prop：需要检查是否存在的属性

**返回值**：返回一个boolean属性的值。

###  其他

- handler.defineProperty(target,property,descriptor)用于拦截对对象的Object.defineProperty()操作
- handler.deleteProperty(target,property)用于拦截对对象属性的delete操作
- handler.getOwnPropertyDescriptor(target,prop)方法是 Object.getOwnPropertyDescriptor()的钩子
- handler.getPrototypeOf(target)是一个代理（Proxy）方法，当读取代理对象的原型时，该方法就会被调用
- handler.isExtensible(target)用于拦截对对象的Object.isExtensible()
- .......





#  Reflect

**概念**：为**操作对象**而提供的新API

**作用（WHY）**：

- 将Object对象的属于语言内部的方法放到Reflect对象上，即从Reflect对象上拿Object对象内部方法
- 将使用老Obeject方法报错的情况，改为返回false
- 让Object操作变为函数行为
- Reflect与Proxy是相辅相成的，在Proxy上有的方法，在Reflect上就一定有（**确保对象的属性能正确赋值，广义上讲，即确保对象的原生行为能够正常进行，这就是Reflect的作用**）

| 方法                               | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| **Reflect.apply()**                | 对一个函数进行调用操作，同时可以传入一个数组作为调用参数     |
| **Reflect.construct()**            | 对构造函数进行`new`操作，相当于执行`new target(...args)`     |
| Reflect.defineProperty()           | 和Object.defineproperty()类似                                |
| Reflect.deleteProperty()           | 相当于执行delete target[name]                                |
| **Reflect.get()**                  | 获取对象上某个属性的值                                       |
| Reflect.getOwnPropertyDescriptor() | 类似于Object.getOwnPropertyDescriptor()                      |
| Reflect.getPrototypeOf()           | 类似于Object.getPrototypeOf()                                |
| **Reflect.has()**                  | 判断一个对象是否存在某个属性，和`in`运算符的功能完全相同     |
| Reflect.isExtensible()             | 类似于Object.isExtensible()                                  |
| Reflect.ownKeys()                  | 返回一个包含所有自身属性（不包含继承属性）的数组             |
| Reflect.preventExtensions()        | 类似于Object.preventExtensions()，返回一个布尔值             |
| **Reflect.set()**                  | 将值分配给属性的函数。返回一个布尔值，如果更新成功，则返回true |
| Reflect.setPrototypeOf()           | 类似于Object.setPrototypeOf()                                |





#  Promise对象

##  介绍

定义：Promise就是一个对象，用来传递异步操作的消息。代表了某个未完成，但预计将来会完成的操作（异步操作），并且为这个操作提供统一的API，可供进一步处理。（不是新语法，而是一种新写法）

拥有三个状态：

- `pending`：初始值
- `fulfilled`：代表操作成功
- `rejected`：代表操作失败

**特点**：

1. 对象的状态不受外界影响。除了异步操作的结果，其他行文都不能改变Promise对象的状态；
2. 一旦状态改变就固定下来。当状态从`pending`转变则之后状态会固定不变。并且当这种状态变化发生，`Promis.then`绑定的函数就会被调用。

**作用**：可以将异步操作的流程表达出来，避免了层层嵌套的回调函数。并且提供了统一的接口，使控制异步操作更加容易。

缺陷：

1. 无法取消Promise。一旦**新建Promise就会立即执行**，无法中途取消；
2. 如果不设置回调函数，Promise内部抛出的错误不会反应到外部；
3. 当处于`pending`状态时，无法得知进展到什么地步（刚开始还是即将完成）。

##  用法

```js
const promise = new Promise(function(resolve,reject) {
    // ...some code
    if(/*异步操作成功*/){
       resolve(value)
    }else{
        reject(error)
    }
})
```

`Promise`接受一个**函数**作为参数，该函数的两个参数分别是`resolve`和`reject`(是两个函数，由 JavaScript 引擎提供，不用自己部署)。

- `resolve`函数：将Promise对象的状态从“未完成”变为“成功”，在异步操作成功时调用，并将异步操作的结果作为参数传递出去
- `reject`函数：将Promise对象的状态从“未完成”变为“失败”，在异步操作失败时调用，并将异步操作报出的错误作为参数传递出去

**Promise实例生成**后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。`then`方法接受两个回调函数作为参数。第一个回调函数在Promise对象变为`resolved`状态时调用，第二个回调函数（可选）是Promise对象变为`rejected`状态时调用。

```js
promise.then(function(value) {
    //success
},function(error){
    //failure
})
```

##  基本API

###  .then()

对`Promise`添加回调，并返回一个**新的`Promise`实例**（链式写法的基础），且异步操作的返回值作为参数传入这个新`Promise`的`resolve`函数。

###  .catch()

用于指定发生错误时的回调函数。`Promise`对象的错误会一直向后传递，直到被捕获。即其总会被下一个`catch`捕获。`.catch === .then(null,onRejected)`

###  .all()

用于将多个`Promise`实例，包装成一个新的`Promise`实例。此方法接受一个**数组**，数组中的对象均为`Promise`实例。包装的`Promise`的状态由数组中的`Promise`决定。这多个`Promise`是同时开始、并行执行。

```javascript
var p = Promise.all([p1,p2,p3]);
```

###  .race()

同样将多个`Promise`实例，包装成一个新的`Promise`实例。当p1,p2,p3中有一个实例发生改变，p的状态跟着改变，并把第一个改变状态的`Promise`的返回值，传给p的回调函数。（相当于哪个`Promise`状态先改变就取其结果，类似赛跑取第一名）

```javascript
var p = Promise.race([p1,p2,p3]);
```

###  .resolve()

可以看作`new Promise()`的快捷方式。其有两个作用：

1. 会让`Promise`对象立即进入`resolved`状态，并将结果传给`then`函数
2. 将`thenable`对象（即带有`then`方法的对象）转换为`Promise`对象

```javascript
Promise.resolve('123');
//等同于
new Promise(function(resolve){
    resolve('123');
})
```

###  .reject()

作用与`Promise.resolve()`类似。这段代码会让这个`Promise`对象立即进入`rejected`状态，并将错误的对象传递给`then`指定的`onRejected`回调函数。

###  .finally()

`finally()`方法用于指定不管Promise对象最后状态如何，都会执行的操作。

```javascript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```





#  手写Promise

按照[Promise/A+](https://promisesaplus.com/)规范，手写一个Promise

##  Promise的基本框架搭建

###  Promise代码基本结构

实例化Promise对象时传入一个函数作为执行器`executor`，其有两个参数（`resolve`和`reject`）分别将结果变为成功态和失败态。其中包含3个属性，分别为`state`、`value`、`reason`。

`state`：保存Promise对象的状态，可以为：等待状态 `pending`、成功状态`resolved`、失败状态`rejected`。

并且要求Promise在实例化后立即执行，因此，基本结构可以写成：

```js
function MyPromise(executor) {
    var _this = this;
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    
    function resolve(value){
        //some code
    }
    
    function reject(reason){
        //some code
    }
    //这样的话，实例化一个MyPromise对象时，直接运行executor函数
    executor(resolve,reject)
}
```

###  then方法定义在原型上

每一个Promise实例都有一个`then`方法，用来处理异步返回的结果，因此将`then`方法定义在原型上。

```js
MyPromise.prototype.then = function(onFulfilled,onRejected){
    //some code
}
```

此时，Promise的基本框架已经搭好，接下来对其功能进行完善。

##  实现Promise的状态改变

我们知道`resolve`函数在异步操作成功时，将Promise对象的状态从“未完成”变为“成功”，并将异步操作的结果作为参数传递出去。`reject`函数：在异步操作失败时将Promise对象的状态从“未完成”变为“失败”，并将异步操作报出的错误作为参数传递出去。并且状态的改变只能是`pending->resolved`和`pending->rejected`，以及变化后状态固定。

```js
function resolve(value){
    //当状态为pending时才可以改变状态
    if(_this.state === 'pending'){
        _this.value = value;
        _this.state = 'resolved'
    }
}

function reject(reason){
    //当状态为pending时才可以改变状态
    if(_this.state === 'pending'){
        _this.reason = reason;
        _this.state = 'rejected'
    }
}
```

##  基本实现then方法

因为当Promise状态发生改变后，都会调用`then`方法，不论成功或失败。这个函数的参数都是可选的，当参数不为函数则忽略。如果传入的参数为函数则执行。

```js
MyPromise.prototype.then = function(onFulfilled,onRejected){
    if(this.state === 'resolved'){
        if(typeof onFulfilled === 'function'){
            onFulfilled(this.value)
        }
    }
    if(this.state === 'rejected'){
        if(typeof onRejected === 'function'){
            onRejected(this.reason)
        }
    }
}
```

##  完善Promise的功能

目前为止，已经可以实现Promise的基本功能，但仍有某些性质需要完善，比如异步运行，链式调用，返回新的Promise等，接下来便是对Promise的逐步完善。

###  实现异步执行

现在的MyPromise还不支持异步代码。什么意思呢？来举例说明

```js
let p1 = new MyPromise((resolve,reject) => {
    resolve(1)
});
p1.then(data => console.log(data))

let p2 = new MyPromise((resolve,reject) => {
    setTimeout(() => {
        resolve(1)
    },0)
});
p2.then(data => console.log(data))  //没有输出
```

由于代码的执行顺序是先执行同步代码再执行异步代码，`then()`方法在`state`属性改变前就已执行，因此上方代码中的`p2`没有输出。

鉴于这个原因，我们参考**发布订阅者模式**。MyPromise的`state`作为发布者，各个`onFulfilled`、`onRejected`作为订阅者。调用`then()`时，先将`onFulfilled`、`onRejected`函数保存起来。当异步操作完成，`state`状态变化，通知订阅者并调用。

因此，对MyPromise进行改进。

首先，在MyPromise的构造函数中新增两个属性，`onFulfilledFunc`和`onRejectedFunc`分别用来保存`onFulfilled`和`onRejected`函数，因为可能有多个函数，所以这两个属性均为数组。

```js
function MyPromise(executor) {
    var _this = this;
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    this.onFulfilledFunc = [];//保存状态变为成功时调用的回调函数
    this.onRejectedFunc = [];//保存状态变为失败时调用的回调函数
    // other code...
}
```

其次，调用`then()`时将`onFulfilled`和`onRejected`函数放入对应的数组中。

```js
MyPromise.prototype.then = function(onFulfilled,onRejected){
    //Promise中有异步代码
    if(this.state === 'pending'){
        if(typeof onFulfilled === 'function'){
            this.onFulfilledFunc.push(onFulfilled)//保存
        }
        if(typeof onRejected === 'function'){
            this.onRejectedFunc.push(onRejected)//保存
        }
    }
    //Promise中为同步代码,立即执行
    if(this.state === 'resolved'){
        if(typeof onFulfilled === 'function'){
            onFulfilled(this.value)
        }
    }
    //Promise中位同步代码，立即执行
    if(this.state === 'rejected'){
        if(typeof onRejected === 'function'){
            onRejected(this.reason)
        }
    }
}
```

最后，当异步操作完成，改变`state`状态，并调用对应数组中所有函数。

```js
function MyPromise(executor) {
    // other code...
    function resolve(value){
    //当状态为pending时才可以改变状态
    if(_this.state === 'pending'){
        _this.value = value;
        _this.state = 'resolved';
        _this.onFulfilledFunc.forEach(fn => fn(value))
    }
}

function reject(reason){
    //当状态为pending时才可以改变状态
    if(_this.state === 'pending'){
        _this.reason = reason;
        _this.state = 'rejected';
         _this.onRejectedFunc.forEach(fn => fn(reason))
    }
}
}
```

此时，已经支持异步操作。

```js
//测试
let p1 = new MyPromise((resolve,reject) => {
    resolve(1)
});
p1.then(data => console.log(data))  //1

let p2 = new MyPromise((resolve,reject) => {
    setTimeout(() => {
        resolve(2)
    },0)
});
p2.then(data => console.log(data))  //2
```

###  保证then方法返回一个新的Promise

使用Promise进行异步编程的最重要的原因就是其规避了嵌套地狱，改为了链式调用。要想实现链式调用，那么`then()`返回一个Promise对象是前提。

```js
MyPromise.prototype.then = function (onFulfilled, onRejected) {
    let promise2 = new Promise((resolve, reject) => {
    //some code
    }
    return promise2;
};
```

###  实现Promise的链式调用

Promise链式调用的规则为：

1. 每个`then`方法都返回一个新的Promise对象
2. 如果`then`方法中显式的返回了一个Promise对象，就不做任何变动，直接返回其结果
3. 如果`then`方法中返回的是一个普通值（Number、String等）就将这个值包装为一个新Promise对象并返回
4. 如果`then`方法中没有return语句，则视为返回一个包装Undefined的Promise对象
5. 如果`then`方法中出现异常，则调用失败态方法（reject）跳转到下一个`then`的`onRejected`函数
6. 如果`then`方法没有传入任何回调，则继续向下传递值

因为要实现的这些功能较为复杂，抽离一个方法来实现。

```js
/**
 * 解析then返回值与新Promise对象
 * @param {Object} promise2 新的Promise对象 
 * @param {*} x 上一个then的返回值
 * @param {Function} resolve promise2的resolve
 * @param {Function} reject promise2的reject
 */
function resolvePromise(promise2, x, resolve, reject) {
    //...
}
```

首先需要判断参数`promise2`与`x`是否相等，若相等则会发生循环引用，无法正常执行。

```js
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 == x){
        reject(new TypeError('Promise发生了循环引用'))
    }
}
```

然后按参数`x`的类型分别处理。当`x`是一个Promise，就直接执行；当`x`是一个对象或函数，再进一步处理；剩下情况则为对普通值的处理。

```js
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 == x){
        reject(new TypeError('Promise发生了循环引用'))
    }
    
    if(x!=null && (typeof x==='object'||typeof x==='function')){
        //x为对象或函数
    }else{
        //x为普通值
        resolve(x)
    }
}
```

如果`x`是一个对象，我们就以是否有`then`方法判断其是否为Promise对象。若报错，则直接将`promise2`转为失败态。若有`then`，且为一个函数，就可认定`x`是Promise对象，之后使用`x`作为`this`调用`then`方法。

```js
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 == x){
        reject(new TypeError('Promise发生了循环引用'))
    }
    
    if(x!=null && (typeof x==='object'||typeof x==='function')){
        //x为对象或函数
        try{
            let then = x.then；
            if(typeof then === 'function'){
                //then是function，那么执行Promise
                then.call(x, y =>{
                    resolve(y)
                },(r) => {
                    reject(r)
                })
            }
        }catch(e){
            reject(e)
        }
    }else{
        //x为普通值
        resolve(x)
    }
}
```

若Promise对象转为成功态或失败态时传入了一个Promise对象，此时应使用递归的方法继续执行，直至Promise全部执行完。

```js
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 == x){
        reject(new TypeError('Promise发生了循环引用'))
    }
    
    if(x!=null && (typeof x==='object'||typeof x==='function')){
        //x为对象或函数
        try{
            let then = x.then；
            if(typeof then === 'function'){
                //then是function，那么执行Promise
                then.call(x, y =>{
                    //递归调用，传入y若是Promise对象，继续循环
                    resolvePromise(promise2, y, resolve, reject)
                },(r) => {
                    reject(r)
                })
            }
        }catch(e){
            reject(e)
        }
    }else{
        //x为普通值
        resolve(x)
    }
}
```

最后，由于我们自己的代码是同步执行，那么使用setTimeout函数来变为异步执行。

```js
setTimeout(() => {
    try {
        let x = onFulfilled(value);
        resolvePromise(promise2, x, resolve, reject);
    } catch (e) {
        reject(e);
    }
},0);
```

##  完整代码

```js
function MyPromise(executor) {
    let _this = this;
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    this.onFulfilledFunc = [];//保存状态变为成功时调用的回调函数
    this.onRejectedFunc = [];//保存状态变为失败时调用的回调函数

    function resolve(value){
        //当状态为pending时才可以改变状态
        if(_this.state === 'pending'){
            _this.value = value;
            _this.state = 'resolved';
            _this.onFulfilledFunc.forEach(fn => fn(value))
        }
    }

    function reject(reason){
        //当状态为pending时才可以改变状态
        if(_this.state === 'pending'){
            _this.reason = reason;
            _this.state = 'rejected';
            _this.onRejectedFunc.forEach(fn => fn(reason))
        }
    }
    //这样的话，实例化一个MyPromise对象时，直接运行executor函数
    //executor执行可能出错
    try{
      executor(resolve, reject);
    } catch (err) {
      reject(err);
    }
}

MyPromise.prototype.then = function(onFulfilled,onRejected){
    let promise2 = new MyPromise((resolve,reject) => {
        //Promise中有异步代码
        if(this.state === 'pending'){
            if(typeof onFulfilled === 'function'){
                this.onFulfilledFunc.push(() => {
                    // 异步
                    setTimeout(() => {
                        try {
                            let x = onFulfilled(this.value);
                             // resolvePromise函数，处理自己return的promise和默认的promise2的关系
                            resolvePromise(promise2, x, resolve, reject);
                        } catch (e) {
                            reject(e);
                        }
                    }, 0);
                })//保存
            }
            if(typeof onRejected === 'function'){
                this.onRejectedFunc.push(() => {
                    setTimeout(() => {
                        try{
                            let x = onRejected(this.reason);
                             // resolvePromise函数，处理自己return的promise和默认的promise2的关系
                            resolvePromise(promise2, x, resolve, reject)
                        }catch(e){
                            reject(e)
                        }
                    },0)
                })//保存
            }
        }
        
        if(this.state === 'resolved'){
            if(typeof onFulfilled === 'function'){
                // 异步
                setTimeout(() => {
                    try {
                        let x = onFulfilled(this.value);
                         // resolvePromise函数，处理自己return的promise和默认的promise2的关系
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (e) {
                        reject(e);
                    }
                }, 0)
            }
        }
        
        if(this.state === 'rejected'){
            if(typeof onRejected === 'function'){
                // 异步
                setTimeout(() => {
                    try{
                        let x = onRejected(this.reason);
                         // resolvePromise函数，处理自己return的promise和默认的promise2的关系
                        resolvePromise(promise2, x, resolve, reject)
                    }catch(e){
                        reject(e)
                    }
                },0)
            }
        }
    });
    return promise2
};


/**
 * 解析then返回值与新Promise对象
 * @param {Object} promise2 新的Promise对象 
 * @param {*} x 上一个then的返回值
 * @param {Function} resolve promise2的resolve
 * @param {Function} reject promise2的reject
 */
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 === x){
        reject(new TypeError('Promise发生了循环引用'))
    }

    if(x!=null && (typeof x==='object'||typeof x==='function')){
        //x为对象或函数
        try{
            let then = x.then;
            if(typeof then === 'function'){
                //then是function，那么执行Promise
                then.call(x, y =>{
                    //递归调用，传入y若是Promise对象，继续循环
                    resolvePromise(promise2, y, resolve, reject)
                },(r) => {
                    reject(r)
                })
            }
        }catch(e){
            reject(e)
        }
    }else{
        //x为普通值
        resolve(x)
    }
}
```











#  Generator函数

##  介绍

`Generator`函数是ES6提供的一种异步编程的解决方案，相比于Promise，语义更加清晰。

`Generator`函数是一个**状态机**，也是一个**遍历器对象**生成函数，封装了多个内部状态。执行`Generator`函数会返回一个**遍历器对象**，这个对象可以依次遍历`Generator`函数内部的每一个状态。调用方法与普通函数一样，不同的是，调用`Generator`函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象`{value: '***', done: true/false}`。`Generator`函数执行需要`next`方法，当遇到`yield`暂停运行或`return`停止运行。

###  形式

两个特征：

- ​	`function`关键字与函数名之间有一个星号`*`;
- ​    函数体内部使用`yield`表达式，定义不同的内部状态。

```javascript
//例如：
function* helloworld() {
    yield 'hello';
    yield 'world';
    return 'ending';
}
```

###   使用方法

Generator函数的**调用**方法与普通函数一样，但调用Generator函数后，该函数并不执行，会**返回**一个指向内部状态的**指针对象**，即**遍历器对象**。必须调用遍历器对象的next方法才能使 指针移向下一个状态。

##  重要语法

###  yield表达式、return、next之间的关系

`yield`表达式是暂停标志，`return`为结束标志，`next`可以看作开始标志。

####  运行逻辑

1. 遇到`yield`表达式，就暂停执行后面的操作，并将紧跟`yield`后的表达式的值作为返回形象的`value`属性值；
2. 下一次调用`next`方法时，再继续往下执行，直到遇到下一个`yield`表达式；
3. 如果未遇到新的`yield`表达式，就一直运行到函数结束，直到`return`语句为止，并将`return`语句后面的表达式的值作为返回对象的`value`属性值；
4. 如果该函数没有`return`语句，则返回对象的`value`属性值为`undefined`。

####  注意

`yield`表达式只能用在`Generator`函数里，用在其他地方都会报错。

`yield`表达式本身没有返回值，或者说总是返回`undefined`。`next`方法可以带有一个参数，该参数会被当作上一个`yield`表达式的返回值。



###  for···of循环

`for...of`循环可以自动遍历`Generator`函数运行时生成的`Iterator`对象，且此时不再需要调用`next`方法。当返回对象的`done`属性值为`true`，循环就会中止，且不包含该返回对象（也就是`return`后面的内容不返回）。

```javascript
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```

##  基本API

###  .throw()  ***

`Generator`函数返回的遍历器对象，都有一个`throw`方法，可以在函数体外抛出错误，然后在`Generator`函数体内捕获。`throw`方法可以接受一个参数，该参数会被`catch`语句接收，建议抛出`Error`对象的实例。

###  .return()

`return()`方法可以返回给定的值，并且终结遍历`Generator`函数。即，return的参数值覆盖本次yield语句的返回值，并提前终结遍历。



####  next()、throw()、return()比较

- `next()`将`yield`表达式替换成一个值；
- `throw()`将`yield`表达式替换成一个`throw`语句；
- `return()`将`yield`表达式替换成一个`return`语句。

###  yield* 表达式

用来在一个`Generator`函数里执行/调用另一个`Generator`函数。

##  应用

Generator可以暂停函数执行，返回任意表达式的值，因此其具有多种应用场景。

- **异步操作的同步化表达**
- 控制流管理
- 部署Iterator接口
- 作为数据结构

###  异步应用

整个 Generator 函数就是一个封装的异步任务，或者说是异步任务的容器。异步操作需要暂停的地方，都用`yield`语句注明。

注意：Generator函数将异步操作表示得很简洁，但流程管理不方便。

为了让Generator函数自动执行，可以分别用Thunk函数（1960）和co模块（2013.6）实现。





#  async函数

##  介绍

###  定义与表现形式

ES2017标准引入了async函数，使得异步操作更加方便。async函数本质上就是Generator的**语法糖**（将**Generator函数**和**自动执行器**包装在一个函数里）。

```javascript
const fs = require('fs');
//读取文件
const readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};
//Generator函数
const gen = function* () {
  const f1 = yield readFile('/etc/fstab');
  const f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
//async函数
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

在形式上，`async`函数就是将 Generator 函数的星号（`*`）替换成`async`，将`yield`替换成`await`。

###  优点

1. 内置执行器：async函数可以自动执行，在表现形式上，只需调用就会自动执行。不需要Generator函数使用next函数，或co模块。

   ```js
   var result = asyncReadFile()
   ```

2. 更好的语义：async与await拥有更清楚的语义。**async**表示函数里**有异步操作**，**await**表示紧跟在后面的表达式**需要等待结果**。

3. 更广的适用性：async函数的await命令后面可以跟Promise对象和原始类型的值（这个直接量会被Promise.resolve()封装成Promise对象，这时等同于同步操作）。

##  用法

###  前言

####  async

async用来表示函数中有异步操作，返回的是一个Promise对象，如果是return一个直接量，async会把这个直接量通过Promise.resolve()封装成Promise对象。

因为返回的是一个Promise对象，若最外层不能用await获取其返回值时，应当使用then()来处理这个Promise对象。

####  await

await用来表示紧跟在后面的表达式需要等待结果。即**等待一个返回值**（Promise对象、普通函数调用、直接量等）。

- 若等到不是Promise对象，那么await表达式的运算结果就是等待获取的东西；
- 若等到的是Promise对象，则会阻塞后面的代码，等待Promise对象达到"resolved"或“rejected”状态，并将已完成的值作为await表达式的运算结果。

###  流程

1. 调用async函数，直接执行；
2. 遇到await关键字，如果其后跟着Promise对象，则阻塞代码并等待Promise对象运算结果的返回；若不是，则相当于执行同步代码；
3. 循环第2步，知道async函数执行完毕。

###  注意

- await 命令后面的 Promise 对象，运行结果可能是 rejected，所以最好把 await 命令放在 **try...catch 代码块**中。

- 如果确实希望多个请求**并发**执行，可以使用 **Promise.all** 方法。

  ```js
  async function dbFuc(db) {
    let docs = [{}, {}, {}];
    let promises = docs.map((doc) => db.post(doc));
  
    let results = await Promise.all(promises);
    console.log(results);
  }
  
  // 或者使用下面的写法
  
  async function dbFuc(db) {
    let docs = [{}, {}, {}];
    let promises = docs.map((doc) => db.post(doc));
  
    let results = [];
    for (let promise of promises) {
      results.push(await promise);
    }
    console.log(results);
  }
  
  ```

  

#  XMLHttpRequest

定义：XMLHttpRequest，用于与服务器交互数据，是Ajax功能实现所依赖的对象。

功能：使用XMLHttpRequest对象可以与服务器交互，能够从URL获取数据，而无需让整个页面刷新。

##  使用

首先需要实例化一个XHR对象。

```js
let xhr = new XMLHttpRequest()
```

###  发送请求

####  open()

`open()`用以初始化一个请求。

```js
xhr.open(method, url, async, user, password)
```

接收5个参数：

1. 第一个参数用于指定发送请求的方式，是一个字符串。
2. 第二个参数是URL，用于指定请求数据的地址，满足同源策略。
3. 第三个参数是一个布尔值，表示是否异步发送请求。
4. 第四个和第五个参数在URL受密码保护时使用。

####  send()

`send()`方法用于发送HTTP请求。如果是异步请求，则此方法会在请求发送后立即返回；如果为同步请求，此方法接收到响应后才返回。

`send()`方法接收一个参数，即要作为请求主体发送的数据

###  接收响应

一个完整的HTTP响应由状态码、响应头集合和响应主体组成。主要的属性有：

```
responseText: 作为响应主体被返回的文本(文本形式)
responseXML: 如果响应的内容类型是'text/xml'或'application/xml'，这个属性中将保存着响应数据的XML DOM文档(document形式)
status: HTTP状态码(数字形式)
statusText: HTTP状态说明(文本形式)
```

通过XHR对象的readyState属性判断请求/响应过程的当前活动阶段。

```
0(UNSENT):未初始化。尚未调用open()方法
1(OPENED):启动。已经调用open()方法，但尚未调用send()方法
2(HEADERS_RECEIVED):发送。己经调用send()方法，且接收到头信息
3(LOADING):接收。已经接收到部分响应主体信息
4(DONE):完成。已经接收到全部响应数据，而且已经可以在客户端使用了
```

####  同步请求

一旦send()返回，仅需要检查XHR对象的status和responseText属性即可。但应避免发送同步请求。

####  异步请求

如果需要接收的是异步响应，这就需要检测XHR对象的readyState属性。理论上，只要readyState属性值由一个值变成另一个值，都会触发一次readystatechange事件。可以利用这个事件来检测每次状态变化后readyState的值。

```js
//因为兼容性问题，在调用open()之前应指定onreadystatechange()
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4){
        if(xhr.status == 200){
            alert(xhr.responseText);
        }
    }
}
```





#  axios

##  概念

定义：axios是一个基于promise的HTTP库，可以用在浏览器和node.js中。本质上就是对原生XHR的封装，不过是Promise的实现形式。

特性：

- 在浏览器创建XMLHttpRequests
- 在node.js则创建HTTP请求
- 支持Promise API
- 支持拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御XSRF

##  对比

|             |                             定义                             |              优点               |                             缺点                             |
| :---------: | :----------------------------------------------------------: | :-----------------------------: | :----------------------------------------------------------: |
|  原生ajax   | 一种创建交互式、快速动态网页的应用的网页开发技术（不需重新加载整个网页，对其中某部分进行更新） | 1、局部更新<br />2、原生JS支持  |      1、可能破坏浏览器后退功能<br />2、容易造成回调地狱      |
| jQuery-Ajax |       Ajax的jQuery实现形式，使用jQuery对Ajax进行了封装       | 1、语义更清晰<br />2、支持jsonp |                       容易造成回调地狱                       |
|    fetch    | 另一种HTTP数据请求方式，是XMLHttpRequest的一种替代方案，是原生JS |         解决了回调地狱          | 1、API 偏底层，需要封装<br />2、默认不带Cookie，需要手动添加;<br />3、浏览器支持情况不是很友好，需要第三方的ployfill |
|    axios    |      一个基于promise的HTTP库，可以用在浏览器和node.js中      |    语义清晰、解决回调地狱……     |                                                              |





# 模块机制

`JavaScript`的模块机制其实是借鉴的其他程序设计语言的。`JavaScript`在设计之初并没有提供类似模块的功能，随后在ES5时代出现了各种模拟类似的功能模范，并且在ES6中新增了模块机制。模块机制就是将逻辑上相关的代码组织到同一个包内，包内是一个相对独立的作用域，不用担心命名冲突等等, 当需要在外部使用时直接import相应的package即可。

##  无模块化的缺陷

- 污染全局作用域
- 维护成本高
- 依赖关系不明显

##  CommonJS(NodeJS)

CommonJS：通用模块规范，由NodeJS实现。根据CommonJS规范, 一个单独的文件就是一个模块。每一个模块都是一个单独的作用域, 也就是说, 在该模块内部定义的变量, 无法被其他模块读取, 除非定义为global(浏览器中为window)对象的属性。

```js
//模块定义 myModule.js
var name = 'Byron';
function printName(){
    console.log(name);
}
function printFullName(firstName){
    console.log(firstName + name);
}
module.exports = {
    printName: printName,
    printFullName: printFullName
}
//加载模块
var myModule = require('./myModule.js');
myModule.printName();
```

##  AMD（RequireJS）

AMD:Asynchronous Module Definition(异步模块定义), 在浏览器端模块化开发的规范, 不是JavaScript原生支持, RequireJS是AMD规范的具体实现(严格上说是**RequireJS的推广中产生的AMD规范**)。

```js
// 定义模块 myModule.js
define('myModule', ['dependency'], function(){
    var name = 'Byron';
    function printName(){
        console.log(name);
    }
    return {
        printName: printName
    };
});

// 加载模块
require(['myModule'], function (my){
　 my.printName();
});
```

##  CMD（SeaJS）

**CMD**: Common Module Definition通用模块定义, 由国内发展出来, SeaJS是其典型代表, 即SeaJS是通过浏览器对CMD的具体实现

```js
// 定义模块  myModule.js
define(function(require, exports, module) {
  var $ = require('jquery.js');
  var foo = require('foo');
  var out = foo.bar();
  $('div').addClass('active');
  module.exports = out;
});

// 加载模块
seajs.use(['myModule.js'], function(my){

});
```

## ES6模块(import,export)

ES6在语言标准的层面上, 实现了模块功能, 而且实现得相当简单, 完全可以取代`CommonJS`和`AMD`规范, 是浏览器和服务器通用的模块解决方案。

在ES6中每一个模块即是一个文件，在文件中定义的变量，函数，对象在外部是无法获取的。如果你希望外部可以读取模块当中的内容，就必须使用export来对其进行暴露（输出）。（严格模式）

导出：

export：可以有多个，导出的数据在被引入时必须加{}

export default：一个文件中只能出现一次，引入时不需要{}

引入：

import

```js
//模块定义 myModule.js
const name = 'Byron';
function printName(){
    console.log(name);
}
function printFullName(firstName){
    console.log(firstName + name);
}
const myModule = {
    printName: printName,
    printFullName: printFullName
};
export myModule;

//加载模块
import myModule, { printFullName } from './myModule.js';
myModule.printName();
printFullName('Michael');
```

##  对比

|          |                           CommonJS                           |                             AMD                              |                     CMD                     |                         ES6模块                          |
| :------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :-----------------------------------------: | :------------------------------------------------------: |
|  适用于  |                         服务端(node)                         |                           浏览器端                           |       浏览器端（依赖就近、延迟执行）        |                     服务端/浏览器端                      |
| 如何使用 | `module.exports`定义当前模块对外输出的接口；`require`加载模块 | `define`定义一个模块；`require`加载一个模块；`require.config()`指定引用路径 | `define`定义一个模块；`require`加载一个模块 |            `export`导出模块；`import`引入模块            |
| 加载方式 |                             同步                             |                   异步/依赖前置、提前执行                    |           异步/依赖就近、延迟执行           |                           异步                           |
|   优点   |                解决了依赖、全局变量污染的问题                |       适合在浏览器环境中异步加载模块、并行加载多个模块       |           按需加载、节约开发成本            | 在浏览器和服务端都适用；编译时即可完成模块加载，效率更高 |
|   缺点   | 同步加载模式只适用于服务端；在浏览器端，限于网络原因，应使用异步加载 |            不能按需加载、开发成本大、影响用户体验            |                                             |                                                          |



#  原型

JS是一种基于对象的语言，不是真正的面向对象的语言。

用构造函数生成实例对象，无法共享属性和方法。

构造函数，其实就是一个普通函数，但内部使用了`this`变量。对构造函数使用`new`运算符就能生成实例，并且`this`变量会绑定在实例对象上。实例对象会自动含有一个`constructor`属性并指向它们的构造函数。

每一个构造函数都有一个`prototype`属性，指向另一个对象。此对象的所有属性和方法，都会被构造函数的实例继承。

构造函数被设置了一个`prototype`属性，包含一个对象，所有实例对象需要共享的属性和方法，都放在这个对象里；不需要共享的就放在构造函数里面。也就是说，实例对象的属性和方法分为两种，一种是本地的（定义在`this`上，**私有**），另一种是引用的（**公有**）。

![image-20200316170004418](C:\Users\obitesla\AppData\Roaming\Typora\typora-user-images\image-20200316170004418.png)

原型对象是一个内部对象，应当使用`__proto__`访问；`prototype`属性包含一个对象，在这个对象中定义需要被继承的成员。

`prototype`是**函数**才有的属性，`__proto__`是每个**对象**都有的属性

大多数情况下，`__proto__`可以理解为“构造器的原型”，即`__proto__===constructor.prototype`.

`__proto__`就是用来将对象与该对象的原型相连。`__proto__`是原型链，指向实例化的函数原型

![](E:\Download\so_download\upload-images.jianshu.io\6264932-e589472e4f3f6d32.png)





当我们创建一个函数，系统会为这个函数自动分配一个`prototype`指针，并指向它的原型对象。这个对象包含两个部分分别是：`constructor`与`__proto__`,其中`constructor`指向函数自身。

当以实例化这个构造函数是，创建的实例是一个对象，只包含一个`__proto__`指针，指向构造函数的`prototype`原型对象。

![1010198-20170222151629288-2040292378](E:\Download\so_download\images2015.cnblogs.com\1010198-20170222151629288-2040292378.png)

改变了`prototype`之后创建的实例指向了新的`prototype`对象，而之前的依然指向老的`prototype`对象

##  new运算符

**`new`元算符**创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。操作步骤：

1. 创建一个空的简单JavaScript对象（即**`{}`**，创建一个新的内存空间），标记为此构造函数的实例；
2. 构造函数体内部的this指向该内存 ；
3. 执行构造函数体内的代码（给实例添加属性） ；
4. 如果该函数没有返回对象，则返回**`this`**。

如果没有使用 `new` 运算符， **构造函数会像其他的常规函数一样被调用，** 并***不会创建一个对象***。在这种情况下， `this` 的指向也是不一样的。

##  class语法

传统方法生成实例对象

```javascript
function Point(x,y){
    this.x = x;
    this.y = y;
}

Point.prototype.toString = function(){
    return '('+this.x+this.y+')';
}

let p = new Point(1,2)
```

ES6提供了`class`关键字，用来定义类。但这并不`Java`上的类一样。这个关键字可以看作是一个语法糖，只是对传统的写法进行了改写。

```javascript
class Point {
  //构造函数  
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
  //原型上的方法与属性
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

类的数据类型就是函数，类本身就指向构造函数。用法与构造函数一样



`constructor`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。



```js
function A(){}
undefined
A.__proto__ === Function.prototype
true
Function.__proto__ === Object.prototype
false
Function.__proto__
ƒ () { [native code] }
Function.prototype
ƒ () { [native code] }
A.prototype.__proto__
{constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}constructor: ƒ Object()hasOwnProperty: ƒ hasOwnProperty()isPrototypeOf: ƒ isPrototypeOf()propertyIsEnumerable: ƒ propertyIsEnumerable()toLocaleString: ƒ toLocaleString()toString: ƒ toString()valueOf: ƒ valueOf()__defineGetter__: ƒ __defineGetter__()__defineSetter__: ƒ __defineSetter__()__lookupGetter__: ƒ __lookupGetter__()__lookupSetter__: ƒ __lookupSetter__()get __proto__: ƒ __proto__()set __proto__: ƒ __proto__()
A.prototype.__proto__ ===Object.prototype
true
Function.prototype
ƒ () { [native code] }
Function.prototype.__proto__
{constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
Function.prototype.__proto__===Object.prototype
true
Object.__proto__
ƒ () { [native code] }
Object.__proto__===Function.prototype
true
Function.__proto__
ƒ () { [native code] }
Function.__proto__===Function.prototype
true
Object.prototype.__proto__
null
```





#  JS事件机制

定义：事件就是HTML与Javascript发生特定交互的瞬间。

##  事件绑定方式

```js
//第一种：直接绑定在DOM上或使用'on-事件'（只能添加一个事件）
<div onclilck="fun()">click</div>
document.getElementById("xxx").onclick = function(){
    //some code
}

//第二种：标准模式（可以添加多个事件）
document.getElementById("xxx").addEventListener("click",function(e){
    //some code
})

//第三种：只对有'href'属性的标签适用
<a id="btn1" onclick="fun1();" href="javascript:fun2();">btn</a>
```

##  JS事件流

**定义**：事件流描述的是从页面中接收事件的顺序。

**两种顺序**：

- 事件冒泡：事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的节点（文档）

  ![img](https://img-blog.csdn.net/20180408153245766?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM0MDMyNjUy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- 事件捕获：事件开始时由不太具体的节点开始，逐级向内层节点传播，直到最具体的节点最后接收到事件

  ![img](https://img-blog.csdn.net/20180408153346151?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM0MDMyNjUy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###  DOM事件流

####  DOM0级事件流

通过 JavaScript 指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。

####  DOM2级事件流

按照规定，此类事件流包括3个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。

![img](https://img-blog.csdn.net/20180408153537704?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM0MDMyNjUy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

定义了两个方法addEventListener()和removeEventListener()。两者都是接收3个参数：要处理的事件名、事件处理程序的函数和一个布尔值。布尔值为true表示在捕获阶段调用事件处理程序；反之，则在事件冒泡阶段调用事件处理程序。

addEventListener()可以为元素添加多个事件处理程序，触发时会按照添加顺序依次调用。

##  事件委托

**概述**：事件委托就是利用事件冒泡机制，指定一个事件处理程序，管理某一类性的所有事件。

**出现原因**：一般情况下，DOM需要事件处理程序直接添加就好，但可能遇见很多DOM都需要添加事件处理。如果为每一个DOM添加则会大大影响页面的运行性能。因此利用事件冒泡机制，在外层设置事件处理程序，当点击内层所有数据时，最后都会因为冒泡机制传到外层的事件处理程序中。所以这就叫做事件委托，委托外层DOM代为执行事件。

```html
<body>
  <div id="container">
   <ul id="list">
     <li><a href="#">Item 1</a></li>
     <li><a href="#">Item 2</a></li>
     <li><a href="#">Item 3</a></li> 
     <li><a href="#">Item 4</a></li>
     ..................................
     <li><a href="#">Item 1000</a></li>
   </ul>
  </div>
</body>
<script type="text/javascript">
	document.addEventListener("click",function(){
        //some code
    })
</script>
```

但是现在又有了新的问题，点击内层所有DOM都会触发事件。因此需要找到一个方法，规定只有点击某个DOM时才触发事件。

Event对象提供了一个属性叫target，可以返回时间的目标节点，因此使用target确定是触发哪个元素事件（并未定义在这个元素上）。

```html
<body>
  <div id="container">
   <ul id="list">
     <li><a href="#">Item 1</a></li>
     <li><a href="#">Item 2</a></li>
     <li><a href="#">Item 3</a></li> 
     <li><a href="#">Item 4</a></li>
     ..................................
     <li><a href="#">Item 1000</a></li>
   </ul>
  </div>
</body>
<script type="text/javascript">
	document.addEventListener("click",function(e){
        let e = e || window.event;
        let target = e.target || e.srcElement;
        if(target.nodeName.toLowerCase() == 'li'){
            console.log(target.textContent)
        }
    })
</script>
```

**注意**：`event.target`指向我们打算触发事件的元素，`event.currentTarget`指向监听器直接绑定的那个元素。



#  Event Loop

##  前置知识

###  单线程

**概念**：同一时间只能执行一个任务，即任务执行是同步且阻塞的。

**原因**：由于JavaScript的主要用途是与用户互动以及操作DOM，因此就决定了这个语言只能是单线程，这是核心特征，不会改变。

###  调用栈（call stack）

概念：调用栈是解释器（比如浏览器中的 JavaScript 解释器）追踪函数执行流的一种机制。当执行环境中调用了多个函数时，通过这种机制，我们能够追踪到哪个函数正在执行，执行的函数体中又调用了哪个函数（**同步**）。

- 每调用一个函数，解释器就会把该函数添加进调用栈并开始执行。
- 正在调用栈中执行的函数还调用了其它函数，那么新函数也将会被添加进调用栈，一旦这个函数被调用，便会立即执行。
- 当前函数执行完毕后，解释器将其清出调用栈，继续执行当前执行环境下的剩余的代码。
- 当分配的调用栈空间被占满时，会引发“堆栈溢出”错误。

###  Event Table

Event Table可以理解成一张**`事件——回调函数`**对应表。用来存储JavaScript中异步事件（request，setTimeout，IO等）及其对应的回调函数的列表。

###  Event Queue

Event Queue就是**`回调函数队列`**，又名Callback Queue。当Event Table中的事件被触发，事件对应的回调函数就会被push进这个Event Queue，然后等待被执行。

##  事件循环（EventLoop）

**概念**：EventLoop是一个**执行模型/机制**，在不同的地方有不同的实现。浏览器和NodeJS基于不同的技术各自实现了各自的EventLoop。

###  运行流程

![event-loop-process.png](https://segmentfault.com/img/remote/1460000019313035?w=712&h=483)

- 开始，任务先进入 Call Stack
- 同步任务直接在栈中等待被执行，异步任务从 Call Stack 移入到 Event Table 注册
- 当对应的事件触发（或延迟到指定时间），Event Table 会将事件回调函数移入 Event Queue 等待
- 当 Call Stack 中没有任务（js引擎monitoring process进程检查），就从 Event Queue 中拿出一个任务放入 Call Stack

####  异步任务执行顺序

异步任务执行也是有顺序的，不同的异步任务的回调会进入不同的事件队列，分别为宏队列和微队列。

**宏队列，macrotask，也叫tasks。** 一些异步任务的回调会依次进入macro task queue，等待后续被调用，这些异步任务包括：

- **setTimeout**
- **setInterval**
- setImmediate (Node独有)
- requestAnimationFrame (浏览器独有)
- I/O
- UI rendering (浏览器独有)

**微队列，microtask，也叫jobs。** 另一些异步任务的回调会依次进入micro task queue，等待后续被调用，这些异步任务包括：

- process.nextTick (Node独有)
- **Promise**
- Object.observe
- MutationObserver

（注：这里只针对浏览器和NodeJS）

**更为详细的流程（浏览器）**：

1. 执行全局Script同步代码，这些同步代码有一些是同步语句，有一些是异步语句（比如setTimeout等）；
2. 全局Script代码执行完毕后，调用栈Stack会清空；
3. 从微队列microtask queue中取出位于队首的回调任务，放入调用栈Stack中执行，执行完后microtask queue长度减1；
4. 继续取出位于队首的任务，放入调用栈Stack中执行，以此类推，直到直到把microtask queue中的所有任务都执行完毕。**注意，如果在执行microtask的过程中，又产生了microtask，那么会加入到队列的末尾，也会在这个周期被调用执行**；
5. microtask queue中的**所有任务**都执行完毕，此时microtask queue为空队列，调用栈Stack也为空；
6. 取出宏队列macrotask queue中位于队首的任务，放入Stack中执行<font color=red>（宏队列macrotask一次只从队列中取一个任务执行，执行完后就去执行微任务队列中的任务）</font>；
7. 执行完毕后，调用栈Stack为空；
8. 重复第3-7个步骤；
9. 重复第3-7个步骤；

##  Node事件循环

Node.js 在主线程里维护了一个**事件队列，**当接到请求后，就将该请求作为一个**事件**放入这个队列中，然后继续接收其他请求。当主线程空闲时(没有请求接入时)，就开始循环事件队列，检查队列中是否有要处理的事件，这时要分两种情况：如果是非 I/O 任务，就亲自处理，并通过回调函数返回到上层调用（直接返回结果）；如果是 I/O 任务，当 I/O 任务完成以后就执行回调，把请求结果存入事件中，并将该事件重新放入队列中，等待循环，最后释放当前线程，当主线程再次循环到该事件时，就直接处理了。

![img](https://images2015.cnblogs.com/blog/849589/201707/849589-20170709225535712-2041092305.png)

这个图是整个 Node.js 的运行原理，从左到右，从上到下，Node.js 被分为了四层，分别是 **应用层**、**V8引擎层**、**Node API层** 和 **LIBUV层。**

> 应用层：  即 JavaScript 交互层，常见的就是 Node.js 的模块，比如 http，fs
>
> V8引擎层： 即利用 V8 引擎来解析JavaScript 语法，进而和下层 API 交互
>
> NodeAPI层： 为上层模块提供系统调用，一般是由 C 语言来实现，和操作系统进行交互 。
>
> LIBUV层： 是跨平台的底层封装，实现了 事件循环、文件操作等，是 Node.js 实现异步的核心 。

注意：**Node.js 的单线程仅仅是指 JavaScript 运行在单线程中，而并非 Node.js 是单线程。**

举例

```js
console.log('1')

setTimeout(function() {
    console.log('2')
    new Promise(function(resolve) {
        console.log('3')
        resolve()
    }).then(function() {
        console.log('4')
    })
}, 0)

new Promise(function(resolve) {
    console.log('5.')
    resolve()
}).then(function() {
    console.log('6')
    setTimeout(function() {
        console.log('7')
    })
})

console.log('8')
```







#  订阅—发布者模式

定义：一种管理对象及其行为状态之间的关系的得力工具。

实质：多个订阅者同时监听同一个数据对象，当这个数据对象发生变化时执行一个发布事件，通过这个发布事件通知所有订阅者，使它们能够自己改变对数据对象依赖部分的状态。

![发布-订阅者模式](https://img-blog.csdn.net/20180919204129631?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTMwNTQ0MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

作用：降低代码耦合度，便于维护。

##  实现

1. 定义一个发布者对象，该对象有订阅者列表和发布函数两个属性。

   ```js
   function Deliver(){
       //订阅者列表
       this.subscribers = [];
   }
   Deliver.prototype = {
       Constructor: Deliver,
       //消息管理器。当发布者发布消息后，由消息管理器通知订阅者改变状态
       deliver: function(message){
           this.subscribers.forEach(function(item){
               item(message)
           });
           return this
       }
   }
   ```

2. 为发布者对象添加订阅方法和退订方法

   ```js
   //订阅事件
   function subscribe(subscriber,deliver){
       var hasExists = false;
       //判断订阅者的类型是否为函数
       var isFunction = Object.prototype.toString.call(subscriber) === '[object Function]';
    //判断deliver是否为发布者
       if(isFunction && deliver instanceof Deliver){
           hasExists = deliver.subScribers.some(item => item===subscriber);
           //判断订阅者列表中是否已有该订阅者
           if(!hasExists){
               deliver.subscribers.push(subscriber)
           }
       }
   }
   
   
   //退订事件
   function unSubscribe(subscriber,deliver){
       var isFunction = Object.prototype.toString.call(subscriber) === '[object Function]'; 
       if(isFunction && deliver instanceof Deliver){
           hasExists = deliver.subScribers.filter(item => item!==subscriber);
       }
   }
   ```
   
3. 实例化一个发布者并为其添加订阅者

   ```js
   //实例化一个数据发布者
   var messagePost = new Deliver();
   //该发布者有两个订阅者
   subscriber(handle1 , messagePost);
   subscriber(handle2 , messagePost);
   //两个订阅者分别执行的函数为
   function handle1(data){
   	console.log(data);
   }
   function handle2(data){
   	$(".dome").html(data.ip);
   }
   //在数据发生改变的时候发布者发布消息
   websocket.onmessage = function(evet){
   	messagePost.deliver(event.data)
   }
   ```

   





#  内存泄漏

对于持续运行的服务进程，必须及时释放不再用到的内存，否则会影响系统性能或进程崩溃。这样，不再用到的内存，没有及时释放就叫做内存泄漏。

##  识别方法

1. 打开开发者工具，选择Timeline面板
2. 在顶部的Capture字段里面勾选Memory
3. 点击左上角录制按钮
4. 在页面上进行各种操作
5. 一段时间后，点击对话框的stop按钮，面板上就会显示这段时间的内存占用情况

如果占用基本平稳，接近水平，则不存在内存泄漏

反之，则为内存泄漏

##  解决方法

###  WeakMap()与WeakSet()

原理：WeakSet()与WeakMap()里的引用为弱引用，不会被计入垃圾回收机制







#  垃圾回收机制

浏览器的Javascript具有自动垃圾回收机制，执行环境会负责管理代码执行过程中使用的内存。其原理是：垃圾收集器会定期（周期性）找出那些不再继续使用的变量（局部变量），然后释放其内存。

标记无用变量的策略通常有两种：标记清除（常用）与引用计数（不常用）

##  标记清除

最常用的垃圾回收方式。当变量进入环境时（例如声明变量），就将这个变量标记为“进入环境”（从根元素进入可达的变量）；而当变量离开环境时，将其标记为“离开环境”。此时销毁离开环境的值并回收它们所占用的内存空间。

##  引用计数

引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是 1。如果同一个值又被赋给另一个变量，则该值的引用次数加 1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减 1。当这个值的引用次数变成 0 时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。这样，当垃圾回收器下次再运行时，它就会释放那些引用次数为 0 的值所占用的内存。





