继上节节流与防抖文中出现的`func.apply(this, arguments)`，那么这里我们就来解析一下 `apply` 的用法。

## apply

> apply() 方法调用一个具有给定this值的函数，以及作为一个数组（或类似数组对象）提供的参数。 -MDN

### 语法

`func.apply(thisArg, [argsArray])`

它有两个可选的参数，`thisArg` 是在 `func` 函数运行时使用的 `this` 值。 `[argsArray]` 一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 `func` 函数。什么叫类数组对象呢，我的理解是它是有 `length` 但是没有数组的方法。

### 返回值

> 调用有指定this值和参数的函数的结果。

其实我觉得就是返回 `thisArg` 这个东西。。


那么而具体该怎么使用呢？

### 例子

```
let arr1 = [1, 2, 3]
let arr2 = ['a', 'b']
```

现在我有两个数组，我想把一个数组的元素添加到另一个数组中，类似于 `contact` 的效果，但是 `contact` 是返回一个新数组。使用 `push`的话是将另一个数组丢进去而不是数组的元素。当然也可以使用循环之类的或者扩展运算符 `[...arr1, ...arr2]`，但这里我们使用 `apply()` 来实现。


```
arr1.push.apply(arr1, arr2)
```

在这里 `apply` 中的 `this` 值指向的是 `arr1`，而我们传进去的第二个参数是 `arr2`，所以也就是我们 `push` 这个方法 `this` 的指向是 `arr1`，所以 `arr2` 就会被 `push` 到 `arr1` 并且返回的就是 `arr1`。

另一个例子，使用 `Math.max()` 寻找最大值也可以使用 `apply` 来实现，正常情况下 `Math.max()` 接受的参数是一组数值而不能直接传一个数组进去的，而在这里可以直接这样实现：

```
let arr = [3, 2, 4, 6, 1]
Math.max.apply(null, arr)
```
当然也可以这样子（其实我觉得这样好像还方便一点）

```
Math.max([...arr])
```

其实每次在说到 `apply` 的时候不可避免要说到 `call`， 那么 `call` 又是什么呢？

## call

`call` 其实和 `apply` 相差无几，唯一的差别就是接受的第二个参数的区别，`apply` 第二个参数是数组或者类数组对象，而 `call` 则是一个或者多个参数，这就是两者唯一的区别了。

### 语法

```
function.call(thisArg,arg1,arg2,...)
```

### 例子

`call` 用的比较多的就是继承吧，通过调用父构造函数的 `call` 方法来实现继承。

```
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

## bind

`bind` 它是会返回一个新函数的，给我的理解就是传入的 `this` 和 `arg` 参数组成的一个新函数。

> bind()方法创建一个新的函数，在bind()被调用时，这个新函数的this被bind的第一个参数指定，其余的参数将作为新函数的参数供调用时使用。 -MDN


### 语法

```
function.bind(thisArg[, arg1[, arg2[, ...]]])
```

### 返回值

返回的是 `bind` 的一个新的函数。

### 例子

```
this.x = 9;
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX(); // 9

var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```

## 结语

`apply` `call` 和 `bind` 三者理解都能理解，但是使用的话在我日常中来说用的都挺少的，所以这也就牵引出了一个问题，概念性的东西多看看文档和别人的博客都能懂，但是要真正实际使用上手起来还需要一定的时间，只有真正能做到使用才算是掌握了这个东西！
