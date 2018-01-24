---
title: Javascript_prototype
date: 2018-01-23 22:25:53
tags: Javascript
---

### Javascript Prototype

捣鼓过Docker之后，接着开个坑，打算跟着cnblog上面一些文章和MDN的参考资料学习下JS或者说前端方面，这里是第一篇：JS原型。

#### Inheritance

为什么需要继承？ 
举个例子有猫和狗两种动物，可以定义两个类Cat和Dog分别代表这两种动物，而每个动物都有体重，高度，毛色等特征，那么是否需要在各个类里面定义这些属性呢？
这时候就可以通过把共有的属性集成到基类Animal里面，其它动物只需要继承Animal就可以了，减少了重复代码增加了可维护性。

在Javascript中是如何实现继承的呢？ 答案是通过prototype（原型）。

#### Prototype Chain & Inheriting properties

和Java或者C#等语言基于类继承不同，Javascript是基于prototype（原型）来实现继承的。
每个对象都拥有一个私有属性来保存一个连接到另一个对象，这里面的“另一个对象”就是当前对象的原型。  而那个原型对象本身也是对象，也有它自己的原型，所以就会形成一个原型链，知道最后原型是null结束。

在ECMAScript里面，保存连接的私有对象是 [[Prototype]], 但是由于它是私有的不能直接访问。其中一个非标准实现是通过大多浏览器实现的\__proto__属性来访问， 或者在ES2015以后版本可以通过Object.getPrototypeOf() 和 Object.setPrototypeOf() 来获取或者设置原型。

![JS__proto__.png](JS__proto__.png)

上面例子是创建一个Object o，并赋予两个属性a和b。 通过\__proto__获取原型对象，可见通过括号这种方式新建对象的原型是Object.prototype。

![prototype_chain.png](prototype_chain.png)

而上面例子是在第一个例子的基础上，定义一个Object j，赋予属性b和c， 并把o的原型指向j。 当o.c执行时，在当前对象属性找不到c，会到对象的原型上去找，发现o的原型j上有定义c并且值为4，故返回4。 当执行o.b时，由于当前对象已经定义了b的值，故直接返回2而不会继续搜索原型链。 同时j自己也有原型是Object.prototype, 而Object.prototype自己的原型则是null，原型链至此结束。 整条原型链搜索过程可以用符号表示为：
    o ---> j ---> Object.prototype ---> null

假如访问一个属性(举个例子使用代码o.d访问属性名d)，在原型链搜索完后仍未找到，则会返回undefined。

> NOTE: 假如不想访问原型链上面的属性，只访问定义在当前对象的属性，可以使用hasOwnProperty方法或getOwnPropertyNames方法。 hasOwnProperty方法定义在Object.prototype上而默认对象原型会从那里继承，故一般可以直接使用. 下面是一个使用hasOwnProperty的遍历当前对象可枚举属性的例子 以及 使用getOwnPropertyNames的例子：
```javascript
    for (var property in obj) //for...in loop only iterate the enumerable items
    {
        if (obj.hasOwnProperty(property))
        {
            //properities defined under obj directly
        }
    }

    //sample 2 get own peroperties by 'getOwnPropertyNames' function
    Object.getOwnPropertyNames(obj)
    //The Object.getOwnPropertyNames() method returns an array of all properties (including non-enumerable properties except for those which use Symbol) found directly upon a given object.
```


#### Inheriting "methods"

和继承属性类似，方法也可以被继承。在Javascript中，方法可以作为属性添加到对象中。
下面引用MDN的例子：
```javascript    
    var o = {
       a: 2,
       m: function() {
            return this.a + 1;
        } 
    };

    console.log(o.m()); // 3
    // When calling o.m in this case, 'this' refers to o

    var p = Object.create(o);
    // p is an object that inherits from o

    p.a = 4; // creates a property 'a' on p
    console.log(p.m()); // 5
```

这个例子是举例p是从o继承的，所以也能访问o定义的方法。 至于Object.create()方法是除了\__proto__属性外设置对象原型的方法， 它会返回一个新对象，并且新对象是以传入的第一个参数作为原型的，在本例中p是以o作为原型。  而this是会指向构造函数创建的新对象，在这个例子中就是指代p，所以p.m()结果是5。 这个this会在后面的进行更深入的了解。

> NOTE: 通常不显式指定原型的话，都会以Object.prototype作为默认原型，而想要一个新对象不从任何对象继承的话，可以使用 `Object.create(null)`.

### Different prototype chain when create object in different ways

#### Object created with different syntax
```javascript
    var o = {a: 1};
    //o ---> Object.prototype ---> null

    var b = ['a', 'b']
    // b ---> Array.prototype ---> Object.prototype ---> null

    function c() {
        return true;
    }
    // c ---> Function.prototype ---> Object.prototype ---> null
```

简而言之就是使用不同的语法来创建对象，会有不同的效果，原型链也不一样。

#### Create Object with a constructor

在JS语言里面，"函数是第一等公民", 函数本身也是一个对象，而构造函数就是普通的JS函数，只不过加上new操作符。

```javascript
    function Graph() {
        this.vertices = [];
        this.edges = [];
    }

    Graph.prototype = {
        addVertex: function(v) {
            this.vertices.push(v);
        }
    };

    var g = new Graph();
    // g ---> Graph.prototype ---> Object.prototype ---> null
    // Graph ---> Function.prototype ---> Object.prototype ---> null
```
上面例子中，g是new出来的，也就是Graph的一个实例。 值得注意的是Graph函数本身也是对象，所以它本身也会有原型。

> NOTE: 实际上new运算符会： 
1. 创建新的空对象 
2. 把this绑定到新对象 
3. 给新创建的对象添加\__proto__属性，指向构造函数的原型对象 
4. 给函数末尾添加一条return this语句，使被创建的对象从函数返回。


#### With Object.create
这Object.create可以指定新对象的原型， 和上面Inheriting "methods"例子类似，就不赘述。

#### With class keyword
ES2015支持了新的关键字，class，constructor，static，extends，super。这些关键字可以理解为语法糖，背后继承的实现仍然是基于原型链的。 下面是MDN给出的例子，有空再学习下ECS2015的新语法。

```javascript
    'use strict';

    class Polygon {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
    }

    class Square extends Polygon {
    constructor(sideLength) {
        super(sideLength, sideLength);
    }
    get area() {
        return this.height * this.width;
    }
    set sideLength(newLength) {
        this.height = newLength;
        this.width = newLength;
    }
    }

    var square = new Square(2);
```

### References:
[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

[http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)

[http://www.xiaojichao.com/post/jscorev2.html](http://www.xiaojichao.com/post/jscorev2.html)