---
title: Javascript_ExecutionContext
date: 2018-01-26 21:34:19
tags: Javascript
---

## Javascript Execution Context

上一篇了解完原型，这篇将去了解下执行上下文。下面基于参考资料整理的知识点，同时也会加入一些自己的理解。

### Reference:
[http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/)

[http://www.xiaojichao.com/post/jscorev2.html](http://www.xiaojichao.com/post/jscorev2.html)

[http://www.cnblogs.com/TomXu/archive/2012/01/13/2308101.html](http://www.cnblogs.com/TomXu/archive/2012/01/13/2308101.html)

### What is the Execution Context

什么是执行上下文？ 首先顾名思义，执行上下文就是代码执行的环境。 换句话说，当在不同执行上下文里面执行相同的代码，结果有可能不一样。 e.g. this的指向不同导致结果不一样。

ECMAScript分下面几种代码类型，每种代码都是在其执行上下文中求值。
* 全局代码 -- 代码执行的默认环境
* 函数代码 -- 函数体内的代码
* eval代码 -- eval函数内包含的文本

> NOTE： 模块代码 也算一种？


首先默认有一个Global Context，全局代码在这个全局的上下文里执行。 而当函数被调用的时候，就创建了一个新的执行上下文，并被push到栈顶，同时栈顶指向的就是当前激活的执行上下文。

有几点值得注意：
* 调用另一个上下文的上下文被称为caller，而被调用的上下文被称为callee。
* 同一个函数可以创建多个上下文，比如递归调用。

### Execution Context Stack

假如定义执行上下文堆栈为数组：
    ECStack = []

#### global code
那么全局代码执行时，堆栈是这样子的:
    ECStack = [globalContext]

#### function code

```javascript
    function foo()
    {
        bar()
    }

    function bar()
    {
        baz()
    }

    function baz(){}

```
上下文堆栈的变化过程依次为：
```javascript
ECStack = [globalContext]
//进入foo函数时的执行上下文栈， 该执行上下文被激活
ECStack = [globalContext, <foo> functionContext]
ECStack = [globalContext, <foo> functionContext, <bar> functionContext]
ECStack = [globalContext, <foo> functionContext, <bar> functionContext, <baz> functionContext]
//退出baz函数，根据FILO原则，栈顶的baz退出栈
ECStack = [globalContext, <foo> functionContext, <bar> functionContext]
ECStack = [globalContext, <foo> functionContext]
ECStack = [globalContext]
```

> NOTE: 有些对象，比如generator，可能会违反栈的LIFO顺序。一个generator函数可能会挂起其正在执行的上下文，并在结束前将其从栈中删除。一旦generator再次激活，它上下文就被回复，并再次压入栈中：
```javascript
function *gen() {
  yield 1;
  return 2;
}
let g = gen();
console.log(
  g.next().value, // 1
  g.next().value, // 2
);
```

#### eval code
这里有个新概念，eval调用的时候产生的上下文被称为调用上下文(calling context)，区别于evalContext。
```javascript
eval('var x = 10');
 
(function foo() {
  eval('var y = 20');
})();
 
alert(x); // 10
alert(y); // not defined
```

上下文堆栈变化依次为：
```javascript
ECStack = [globalContext]

// eval('var x=10')
ECStack.push(evalContext, callingContext: globalContext)
ECStack.pop()

// foo function call
ECStack.push(<foo> functionContext)

// eval('var y=20')
ECStack.push(evalContext, callingContext: <foo> functionContext)
ECStack.pop()
ECStack.pop()
```
从上下文堆栈可以看出，var x=10会影响calling context 也就是global； 而var y=20会影响function context也就是<foo>； 所以最后在global上下文访问x的值是10，而y是not defined。

#### Key points for ECStack
* 单线程
* 同步执行
* 只有一个global context
* 当递归调用的时候，可以有无限的function context
* 每个函数调用的时候都会创建一个新的execution context（或者说function context）

### Execution Context in Detail 
现在我们知道函数调用会创建一个新的执行上下文，而JS引擎会把这个创建过程分为两个步骤：
1. 创建阶段
2. 代码执行阶段

在创建阶段，当函数被调用且开始执行函数体代码之前：
* 创建变量， 函数， 参数
* 创建作用域链(ES3的说法) 或者是 词法环境（ES5）
* 决定this的指向

而在代码执行阶段：
* 给函数参数赋值，中断或执行代码

可以把execution context理解成一个object，里面有三个主要的属性：
```javascript
    executionContextES3 = {
        'scopeChain': { /* variableObject + all parent execution context's variableObject */ },
        'variableObject': { /* function arguments / parameters, inner variable and function declarations */ },
        'this': {}
    }


    executionContextES5 = {
        ThisBinding: <this value>,
        VariableEnvironment: { ... },
        LexicalEnvironment: { ... },
    }
```




### Variable/Activation Object (ES3) || Variable Environment (ES5)
TBD

### Scope Chain/ Lexical
TBD

### This
TBD

### Hoisting

通常译作变量提升，意思是在变量作用域内，不管变量在哪个位置声明，都会提升到作用域顶部。下面是一个具体例子：
```javascript
//hoisting
console.log(a); //output undefined 而不是error 'variable a is not defined'
var a =1;

//变量提升后等价于下面语句
var a;
console.log(a);
a = 1;
```

我理解这个其实和作用域有关，在代码执行之前会有一个创建阶段, 变量已经在AO里面定义了，所以执行时可以找到。
需要注意的是，在创建阶段，函数声明会比变量声明先创建，如下面这个例子，因为foo的函数引用先创建，再次声明变量foo的时候发现遇到同名的情况，JS引擎会bypass忽略掉新的变量声明。 而与此相对，当function第二次声明的时候，会覆盖原来的声明，故下面例子输出的是'hi'而不是'hello'。
而对于bar，因为这个bar是赋值函数表达式，和函数声明的区别是不会变量提升(hoisting)，所以结果是找不到bar的定义。

```javascript
(function() { //立即执行函数

    console.log(typeof foo); // function pointer
	console.log(foo()) //hi
	console.log(typeof bar); //undefined
    console.log(bar()) // Uncaught TypeError: bar is not a function
    var foo = 'hello',
        bar = function() {
            return 'world';
        };

    function foo() {
        return 'hello';
    }

    function foo() {
        return 'hi'
    }
}());
```