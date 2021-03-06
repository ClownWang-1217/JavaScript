
## 对象

**面向对象编程（Object Oriented Programming，缩写为 OOP)，都有一个标志，就是都有类的概念。常用面向对象的语言，类似(c++，java，c#)，而我们知道ECMAScript并没有类的概念，因此它的对象也相对于那些基于类语言的对象有所不同。ECMA-262 把对象定义为"无序属性的集合,其属性可以包含 基本值，对象或者函数。"严格意义上讲，对象就是一组没有固定顺序的值，而对象的每个属性都有一个名字，而每个名字都映射到一个值。**  

### *一. 理解对象*
- 对象是单个实物的抽象。 
一本书、一辆汽车、一个人都可以是对象，一个数据库、一张网页、一个远程服务器连接也可以是对象。当实物被抽象成对象，实物之间的关系就变成了对象之间的关系，从而就可以模拟现实情况，针对对象进行编程。

- 对象是一个容器，封装了属性（property）和方法（method)。  
属性是对象的状态，方法是对象的行为（完成某种任务）。比如，我们可以把动物抽象为animal对象，使用“属性”记录具体是哪一种动物，使用“方法”表示动物的某种行为（奔跑、捕猎、休息等等）

#### **1. 我们首先来创建一个对象**
```javascript
var person = new Object();
person.name = 'Caleb';
person.age = 25;
person.job = 'Programmer';
person.sayName = function(){
    console.log('my name is ' + this.name + ',i‘m ' + this.age + ' years old and my job is ' + this.job);
}

person.sayName();

```
打印：
```
my name is Caleb,i‘m 25 years old and my job is Programmer
```
解析：  
上面实例使用 ***new*** 创建了 person 对象，包含了四个属性，name，age，job，sayName。其中sayName()方法中用到的 this.name( 将会被解析为 person.name )，同理 this.age,this.job, 都会被解析为  person.age,person.job。  

接下来我们使用字面量的方式创建对象：
```javascript
var person = {
    name:'Caleb',
    age:25,
    job:'Programmer',
    sayName:function(){
        console.log('my name is ' + this.name + ',i‘m ' + this.age + ' years old and my job is ' + this.job);
    }
}
person.sayName();
```
以上这俩种写法，都将创建一个叫做 person 的对象。既然JavaScript不存在真实意义上的类，那如何去实例化一个对象呢？这就要提到构造函数了。:point_down::point_down::point_down::point_down:

 

#### **2. 构造函数**  
面向对象编程的第一步就是生成对象，前面说过，对象是单个实物的抽象。通常需要一个模版，用来表示这一类实物共同的特征，然后这类对象根据这个模版来生成。  
典型的面向对象编程语言（比如 C++ 和 Java），都有“类”（class）这个概念。所谓“类”就是对象的模板，对象就是“类”的实例。但是，JavaScript 语言的对象体系，不是基于“类”的，而是基于构造函数（constructor）和原型链（prototype）。也就是说生成实例对象必定是和构造函数以及原型链相关。

我们来看看以下俩个函数的区别
```javascript
function Person(){

}

function person(){

}
```
解析：   
这俩个函数看起来唯一的区别就是函数名的首字母，一个大写一个小写,你可以称之为普通函数，也可以称之为构造函数。这个先在我们心里有个概念，也就是说在JavaScript世界里所有的函数都可以作为构造函数，也可以是普通函数，只是为了在概念上区分构造和普通函数，从而一般用来当作构造的函数的函数名都是首字母大写。  

#### **3. 那么如何使用构造函数来实例化对象呢？**  

```javascript
var p = new Person();
var p1 = new person();
```
chrome 打开调试可以查看 new Person() 和 new person()的结构如下：
```
new Person()
Person {}__proto__: constructor: ƒ Person()__proto__: constructor: ƒ Object()hasOwnProperty: ƒ hasOwnProperty()isPrototypeOf: ƒ isPrototypeOf()propertyIsEnumerable: ƒ propertyIsEnumerable()toLocaleString: ƒ toLocaleString()toString: ƒ toString()valueOf: ƒ valueOf()__defineGetter__: ƒ __defineGetter__()__defineSetter__: ƒ __defineSetter__()__lookupGetter__: ƒ __lookupGetter__()__lookupSetter__: ƒ __lookupSetter__()get __proto__: ƒ __proto__()set __proto__: ƒ __proto__()


new person()
person {}__proto__: constructor: ƒ person()__proto__: constructor: ƒ Object()hasOwnProperty: ƒ hasOwnProperty()isPrototypeOf: ƒ isPrototypeOf()propertyIsEnumerable: ƒ propertyIsEnumerable()toLocaleString: ƒ toLocaleString()toString: ƒ toString()valueOf: ƒ valueOf()__defineGetter__: ƒ __defineGetter__()__defineSetter__: ƒ __defineSetter__()__lookupGetter__: ƒ __lookupGetter__()__lookupSetter__: ƒ __lookupSetter__()get __proto__: ƒ __proto__()set __proto__: ƒ __proto__()

```
可以看出 除了命名不同，其余结构完全相似。所以可以肯定在实际使用中，构造函数并不区分首字母大小写。 
这个时候我们给对象添加一些属性，如下：
```javascript
p.name = 'zhangsan';
p.sayName = function (){
    console.log('name:'+ this.name);
}
p.sayName();
```
解析：我们给对象p 添加了俩个属性，一个值类型，一个函数类型，其中函数类型中使用了当前对象的name，这个this 的指向则是调用函数的对象，也就是p。最终的结果：
```
// console 输出
name:zhangsan
```
接下来我们看看 new 过程干了些什么事情：  
##### (1). 创建一个构造函数
```javascript
//创建一个构造函数，并且放置一个this.name 
function func(){
    this.name = 'Caleb';
}
//给每个函数默认的属性prototype对象添加一个属性,id = 1
func.prototype.id = 1;
```
解析：  
在ECMAScript 中，每创建一个函数会自带一个属性prototype，可以先不用管他具体的作用，只需要知道prototype就是函数的原型。这时候就有人会问，那什么是原型？好吧，暂且我打个比方大家心里有个概念，原型，就好比上帝在制造人类的时候，与生俱来给人了一些属性，比如 眼睛，鼻子，嘴巴。。等等。而这些与生俱来的属性就统一放在这个prototype里面，如果你想要修改他或者使用它，那么就通过prototype去获取。比如上面的代码中提到的 id，我想给func 添加一个与生俱来的属性叫id，并且给他一个初始值01，那么就可以这样做 func.prototype.id = 1。  
##### (2). 创建一个空对象
```javascript
//创建一个空对象，实际就是开辟了一块新的内存
var obj = {};
```
##### (3). 链接到原型
```javascript
//就是将原型函数(func)所指向的原型对象(func.prototype) 赋值给空对象(obj)中指向原型对象的指针(__proto__)。
obj.__proto__ = func.prototype;
```
解析：  
那么为什么要做这一步操作？在这里解释一下，正如每个函数都有一个属性prototype一样，每个实例化的对象也会有一个指向原型对象的指针，也就是上文所看到的__proto__。以下是在Chrome中调试结果：
```javascript
//输入
obj.__proto__ = func.prototype;
obj

```
```
//打印
func {}
__proto__: id: 1
constructor: ƒ func()
__proto__: Object
```

```javascript
//输入
console.log(obj.id)
```
```
//打印
1
```
解析：  
从上图的调试过程便能看出，当执行了obj.__proto__ = func.prototype 后obj的原型对象(__proto__)中便拥有了id这个属性。而这个id正是从func的原型对象(prototype)中获得。也就是说此时obj已经得到了func的原型。  

##### (4). 绑定this值
```javascript
//就是将原型函数(func)中使用到this的地方全部指向obj
func.apply(obj);
```
解析：  
或者我们逆向思维解释这一步的作用，假如我们不执行这一步，如果我还想通过obj.name使用func中的name：
```javascript
console.log(obj.name);

```
```
//打印
undefined
```
如果我们执行了func.apply(obj) 这一步：
```javascript
func.apply(obj);
console.log(obj.name);

```
```
//打印
Caleb
```
ok，这里就很显然，obj获取了func中的所有this的所有权。那么，这个时候一定会有疑问，obj中__proto__指向的原型属性使用obj.xxx 而 func中的this.xxx 也是通过obj.xxx 来调用，如果此时原型和函数内都拥有同名的属性，这个时候通过obj.xxx调用，到底使用的是哪一个。 
我们来上代码： 
```javascript
//首先我们得给func.prototype添加一个name属性
func.prototype.name = 'sam'
//输入
console.log(obj.name);

```
```
//打印
Caleb
```
解析：  
这里还是调用的是func中this.name的值，那有的人可能会觉得是不是func.prototype.name压根没有起到作用。ok，这里我们做一个测试
```javascript
function func(){
    //这里我们注释掉name,此时是一个没有任何实例属性的构造函数
    //this.name = 'Caleb';
}
//这里给func.prototype添加name
func.prototype.name = 'sam'
var obj = {};
obj.__proto__ = func.prototype;
func.apply(obj);
//输入
console.log(obj.name)
```
```
//打印
sam
```
解析：  
这样我们就充分的论证了，func.prototype.name 确实是给原型对象添加了一个name的属性。
那么为什么打印obj.name 输出的是Caleb？其实是这样的，ECMAScript 每个实例对象在调用属性时候优先会去在自己的实例属性中找，什么是实例属性，就是说凡是构造函数中通过 this. 的属性都是实例属性(下文中也会详细讲解)，类似于传统面向对象语言中的成员。在每个实例化对象所属的内存中都会拿出一部分内存来存储实例属性，所以即便是同一个构造函数，new 出来的不同对象，它的实例属性都指向不同内存地址。  
比如：
```javascript
var obj = new func();
var obj1 = new func();

obj.name = 'newCaleb';
console.log(obj.name);
console.log(obj1.name);

```
```
//打印:
newCaleb
Caleb

```
解析：  
我们通过new func() 创建俩个对象，obj 和 obj1，当我修改了obj.name 但是却没有影响到obj1.
##### (5). 返回新对象
```javascript
//返回空对象 obj
return obj
```
解析：  
返回新对象

##### 以上步骤就是 new 的整个过程，因为是伪代码，省略了个别步骤，只留下关键点。

到现在为止我们掌握了以下几点  
1. 了解什么是对象  
2. 了解构造函数的意义  
3. 了解通过构造函数实例化对象的整个过程  

##### 那么上文中提到过的原型是什么？

### *二. 原型*
其实我不太想讲原型，但是，如果原型的概念彻底懂了，这将会对你的编码以及阅读框架的能力提升很大，但是对于很多人来说不需要理解那么透彻，可是有没有想过，你编码的质量，以及当你出现问题后的解决思路，都是依靠着扎实的基础知识。  

prototype 作为一个特殊的对象属性，存在于每一个函数上，当一个函数通过new关键字“分娩”出其孩子(实例)，这个名为实例的对象就拥有这个函数的prototype对象中所有的成员，从而实现所有实例都共享一组方法或属性。而javascript的类就是通过修改prototype对象，以区别原生对象及其他自定义“类”，比如我们熟悉的，浏览器中，Node这个类就是基于Object修改而来的，Element则是基于Node，而HTMLElement又基于Element。那么针对于我们的业务，我们自己也可以创建自己的类来实现重用与共享。  
#### **1. 属性**  
我们首先来看看原型链中常用的属性类型。
##### **(1). 原型属性**
```javascript
function Dog(){}
Dog.prototype.name = "dog";
Dog.prototype.method = function (){
    console.log('this is my prototype property');
}
```
解析：  
我们在构造函数Dog的原型上添加了俩个属性，一个值类型 name ，另一个函数类型 method.  

##### **(2). 特权属性**  
为了实现实例对象的差异化属性，javascript 允许我们在构造函数内部通过this指定特权属性。如下所示：  
```javascript
function B(){
    this.name = "tom";
    this.method = function (){
    }
}
```
解析：  
我们在构造函数B函数内添加了俩个特权类型属性，一个值类型，一个函数类型，这样的构造函数实例化的对象中特权属性则互不干扰。  

##### **(3). 私有属性**   
有些属性我们不愿意通过实例对象直接获取，如下：  
```javascript
function B(){
    var name = "tom";
    this.getName = function(){
        return name;
    }
}

var b = new B();

console.log(b.name);
console.log(b.getName())

```
```
//输出
undefined
tom
```
解析：  
如上所示，函数内的 name 就是私有属性。只属于当前作用域，并且每个实例化对象都独有一份互不干扰 比如：  
```javascript
function B(){
    var name = "tom";
    this.getName = function(){
        return name;
    }
    this.setName = function(value){
        name = value;
    }
}

var b = new B();
var b1 = new B();
b1.setName('b1');
console.log(b.getName());
console.log(b1.getName());

```
```
//输出
tom
b1
```
解析：  
我们在这里给私有属性添加了一个getName方法 和 一个setName方法，这样我们通过不同实例对象调用不同的setName方法来改变相对应的私有属性 name的值。最终的打印结果，说明不同的实例对象中的私有属性是互不干扰的。  

##### **(4). 类属性** 
```javascript
function A(){

}
A.callName = 'Gavin';

var a = new A();
console.log(a.callName);
console.log(A.callName);
```
可以猜测下这时候打印会是什么？

```
//打印
undefined
Gavin
```
解析：  
第一个通过实例化对象a获取callName 打印结果 undefined，说明并没有获取到，第二个通过函数A获取callName，打印结果Gavin，说明获取成功，这是因为此时的callName只属于函数A，他是一个类属性，并不是实例属性或者特权属性。




























<meta http-equiv="refresh" content="10">
