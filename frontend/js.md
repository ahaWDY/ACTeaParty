# JavaScript面试题

1. js的继承方式有哪些？子类child继承了父类parent之后，parent有一个属性name，这时new一个子类的实例b，如果修改b.name是否会修改父类上的name属性？

1）
方式一：原型链继承
利用原型链的特点进行继承，子类的原型是父类的实例。

```js
function Parent(){
    this.name = 'web前端';
    this.type = ['JS','HTML','CSS'];
}
Parent.prototype.Say=function(){
    console.log(this.name);
}
function Son(){};
Son.prototype = new Parent();
son1 = new Son();
son1.Say();
```

以上例子解释：

①创建一个叫做Parent的构造函数，暂且称为父构造函数，里面有两个属性name、type

②通过Parent构造函数的属性(即原型对象)设置Say方法，此时，Parent有2个属性和1个方法

③创建一个叫做Son的构造函数，暂且称为子构造函数

④设置Son的属性(即原型对象)值为父构造函数Parent的实例对象，即子构造函数Son继承了父构造函数Parent，此时Son也有2个属性和1个方法

⑤对Son构造函数进行实例化，结果赋值给变量son1，即son1为实例化对象，同样拥有2个属性和1个方法

⑥输出son1的Say方法，结果为"web前端"

优点：可以实现继承

缺点：

①因为Son.prototype(即原型对象)继承了Parent实例化对象，这就导致了所有Son实例化对象都一样，都共享有原型对象的属性及方法。代码如下：

```js
son1 = new Son();
son2 = new Son();
son1.type.push('VUE');
console.log(son1.type);//['JS','HTML','CSS','VUE']
console.log(son2.type);//['JS','HTML','CSS','VUE']
```

结果son1、son2都是['JS','HTML','CSS','VUE']

②Son构造函数实例化对象无法进行参数的传递



方式二：借用构造函数继承
在子类型构造函数中调用call()来调用父类型构造函数

```js
function Parent(){
    this.name = 'web前端';
    this.type = ['JS','HTML','CSS'];
}
function Son(){
    Parent.call(this);
}
son1 = new Son();
son1.type.push('VUE');
console.log(son1.type);//['JS','HTML','CSS','VUE']
son2 = new Son();
console.log(son2.type);//['JS','HTML','CSS']
```

以上例子解释：

①创建父级构造函数Parent，有name、type两个属性

②创建子级构造函数Son，函数内部通过call方法调用父级构造函数Parent，实现继承

③分别创建构造函数Son的两个实例化对象son1、son2，对son1的type属性新增元素，son2没有新增，结果不一样，说明实现了独立

优点：

①实现实例化对象的独立性；

②还可以给实例化对象添加参数

```js
function Parent(name){
    this.name = name;
}
function Son(name){
    Parent.call(this,name);
}
son1 = new Son('JS');
console.log(son1);//JS
son2 = new Son('HTML');
console.log(son2);//HTML
```

缺点：

①方法都在构造函数中定义，每次实例化对象都得创建一遍方法，基本无法实现函数复用

②call方法仅仅调用了父级构造函数的属性及方法，没有办法调用父级构造函数原型对象的方法



方式三：组合继承

利用原型链继承和构造函数继承的各自优势进行组合使用

```js
function Parent(name){
    this.name = name;
    this.type = ['JS','HTML','CSS'];
}
Parent.prototype.Say=function(){
    console.log(this.name);
}
function Son(name){
    Parent.call(this,name);
}
Son.prototype = new Parent();
son1 = new Son('张三');
son2 = new Son('李四');
son1.type.push('VUE');
son2.type.push('PHP');
console.log(son1.type);//['JS','HTML','CSS','VUE']
console.log(son2.type);//['JS','HTML','CSS','PHP']
son1.Say();//张三
son2.Say();//李四
```

以上例子解释：

①创建一个叫做Parent的构造函数，里面有两个属性name、type

②通过Parent构造函数的属性(即原型对象)设置Say方法，此时，Parent有2个属性和1个方法

③创建子级构造函数Son，函数内部通过call方法调用父级构造函数Parent，实现继承

④子构造函数Son继承了父构造函数Parent，此时Son也有2个属性和1个方法

⑤分别创建构造函数Son的两个实例化对象son1、son2，传不同参数、给type属性新增不同元素、调用原型对象Say方法

优点：

①利用原型链继承，实现原型对象方法的继承

②利用构造函数继承，实现属性的继承，而且可以参数

组合函数基本满足了JS的继承，比较常用

缺点：

无论什么情况下，都会调用两次父级构造函数：一次是在创建子级原型的时候，另一次是在子级构造函数内部



方式四：原型式继承

创建一个函数，将参数作为一个对象的原型对象

```js
function fun(obj) {
    function Son(){};
    Son.prototype = obj;
    return new Son();
}        
var parent = {
    name:'张三'
}
var son1 = fun(parent);
var son2 = fun(parent);
console.log(son1.name);//张三
console.log(son2.name);//张三
```

以上例子解释：

①创建一个函数fun，内部定义一个构造函数Son

②将Son的原型对象设置为参数，参数是一个对象，完成继承

③将Son实例化后返回，即返回的是一个实例化对象

优缺点：跟原型链类似



方式五：寄生继承

在原型式继承的基础上，在函数内部丰富对象

```js
function fun(obj) {
    function Son() { };
    Son.prototype = obj;
    return new Son();
}
function JiSheng(obj) {
    var clone = fun(obj);
    clone.Say = function () {
        console.log('我是新增的方法');
    }
    return clone;
}
var parent = {
    name: '张三'
}
var parent1 = JiSheng(parent);
var parent2 = JiSheng(parent);
console.log(parent2.Say==parent1.Say);// false
```

以上例子解释：

①再原型式继承的基础上，封装一个JiSheng函数

②将fun函数返回的对象进行增强，新增Say方法，最后返回

③调用JiSheng函数两次，分别赋值给变量parent1、parent2

④对比parent1、parent2，结果为false，实现独立

优缺点：跟构造函数继承类似，调用一次函数就得创建一遍方法，无法实现函数复用，效率较低。

这里补充一个知识点，ES5有一个新的方法Object.create()，这个方法相当于封装了原型式继承。这个方法可以接收两个参数：第一个是新对象的原型对象（可选的），第二个是新对象新增属性，所以上面代码还可以这样：

```js
function JiSheng(obj) {
    var clone = Object.create(obj);
    clone.Say = function () {
        console.log('我是新增的方法');
    }
    return clone;
}
var parent = {
    name: '张三'
}
var parent1 = JiSheng(parent);
var parent2 = JiSheng(parent);
console.log(parent2.Say==parent1.Say);// false
```



方式六：寄生组合继承

利用组合继承和寄生继承各自优势

组合继承方法我们已经说了，它的缺点是两次调用父级构造函数，一次是在创建子级原型的时候，另一次是在子级构造函数内部，那么我们只需要优化这个问题就行了，即减少一次调用父级构造函数，正好利用寄生继承的特性，继承父级构造函数的原型来创建子级原型。

```js
function JiSheng(son,parent) {
    var clone = Object.create(parent.prototype);//创建对象
    son.prototype = clone;      //指定对象
    clone.constructor = son;     //增强对象
}
function Parent(name){
    this.name = name;
    this.type = ['JS','HTML','CSS'];
}
Parent.prototype.Say=function(){
    console.log(this.name);
}
function Son(name){
    Parent.call(this,name);
}
JiSheng(Son,Parent);
son1 = new Son('张三');
son2 = new Son('李四');
son1.type.push('VUE');
son2.type.push('PHP');
console.log(son1.type);//['JS','HTML','CSS','VUE']
console.log(son2.type);//['JS','HTML','CSS','PHP']
son1.Say();//张三
son2.Say();//李四
```

以上例子解释：

①封装一个函数JiSheng，两个参数，参数1为子级构造函数，参数2为父级构造函数

②利用Object.create()，将父级构造函数原型克隆为副本clone

③将该副本作为子级构造函数的原型

④给该副本添加constructor属性，因为③中修改原型导致副本失去默认的属性

优缺点：

组合继承优点、寄生继承的有点，目前JS继承中使用的都是这个继承方法



方式七：ES6中class 的继承

ES6中引入了class关键字，class可以通过extends关键字实现继承，还可以通过static关键字定义类的静态方法,这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（Parent.apply(this)）。ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。

需要注意的是，class关键字只是原型的语法糖，JavaScript继承仍然是基于原型实现的。

```js
       class Person {
            //调用类的构造方法
            constructor(name, age) {
                this.name = name
                this.age = age
            }
            //定义一般的方法
            showName() {
                console.log("调用父类的方法")
                console.log(this.name, this.age);
            }
        }
        let p1 = new  Person('kobe', 39)
        console.log(p1)
        //定义一个子类
        class Student extends Person {
            constructor(name, age, salary) {
                super(name, age)//通过super调用父类的构造方法
                this.salary = salary
            }
            showName() {//在子类自身定义方法
                console.log("调用子类的方法")
                console.log(this.name, this.age, this.salary);
            }
        }
        let s1 = new Student('wade', 38, 1000000000)
        console.log(s1)
        s1.showName()
```

优点：语法简单易懂,操作更方便

缺点：并不是所有的浏览器都支持class关键字

reference: 
https://zhuanlan.zhihu.com/p/105312152
https://segmentfault.com/a/1190000016708006



2）不会。 b.name一般是string类型，属于值类型而不引用类型，会复制值而不是复制地址，因此不会修改到父类上的name属性的值。



2. 对闭包的看法，为什么要用闭包？说一下闭包原理以及应用场景

- 闭包是什么

函数执行后返回结果是一个内部函数，并被外部变量所引用，如果内部函数持有被执行函数作用域的变量。即形成了闭包。

可以在内部函数访问到外部函数作用域。使用闭包，一可以读取函数中的变量，二可以将函数中的变量存储在内存中，保护变量吧被污染。而正是因为闭包会把函数中的变量存储在内存中，会对内存有消耗，所以不能滥用闭包，否则会影响网页性能，造成内存泄漏。当不需要使用闭包时，要及时释放内存，可将内存函数对象的变量赋值为null。



- 闭包的原理

函数执行分为两个阶段（预编译和执行阶段）。

1、在预编译阶段，如果发现内部函数使用了外部函数的变量，则会在内存中创建一个“闭包”对象并保存对应变量值，如果已存在“闭包”，则只需要增加对应属性值即可。

2、执行完后，函数执行上下文会被销毁，函数对“闭包”对象的引用也会被销毁，但其内部函数还持用该“闭包”的引用，所以内部函数可以继续使用“外部函数”中的变量。

利用了函数作用域链的特性，一个函数内部定义的函数会将包含外部函数的活动对象添加到它的作用域链中，函数执行完毕，其执行作用域链销毁，但因内部函数的作用域链仍然在引用这个活动对象，所以其活动对象不会被销毁，直到内部函数被烧毁后才被销毁。



- 闭包的优点

1、可以从内部函数访问外部函数的作用域中的变量，且访问到的变量长期驻扎在内存中，可供之后使用。

2、避免变量污染全局。

3、把变量存到独立的作用域，作为私有成员存在。



- 闭包的缺点

1、对内存消耗有负面影响，因内部函数保存了对外部变量的引用，导致无法被垃圾回收，增大内存使用量，所以使用不当会导致内存泄漏。

2、对处理速度具有负面影响。闭包的层级决定了引用的外部变量在查找时经过的作用域长度。

3、可能获取到意外的值。



- 应用场景

1、模块封装，为了防止变量污染全局

```js
var Fun=(function(){
var foo=0;
function Fun(){}
Fun.prototype.bar=function bar(){
return foo;
}
return Fun;
}());
```

2. 应用场景二:在循环中创建闭包，防止取到意外的值。如下代码，无论哪个元素触发事件，都会弹出3。因为函数执行后引用的i是同一个，而i在循环结束后就是3。

```js
for (var i = 0; i < 3; i++) {
    document.getElementById('id' + i).onfocus = function() {
      alert(i);
    };
}
//可用闭包解决
function Callback(num) {
  return function() {
    alert(num);
  };
}
for (var i = 0; i < 3; i++) {
    document.getElementById('id' + i).onfocus = Callback(i);
}
```



Reference: https://zhuanlan.zhihu.com/p/355302354