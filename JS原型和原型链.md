在平时编码中，我们 `console` 的时候总是会发现有一个这种 `__proto__` 或者 `prototype` 这种东西，那么这些东西究竟是什么呢？那么这篇文章我们就简单的来解释一下这些东西究竟是什么？

首先我们来看一个例子：
```
function Person() {
}
var person = new Person()
person.name = 'tom'
console.log(person) // { name: tom }

```

首先我们先创建一个构造函数，然后用 `new` 创建了一个实例化对象，接着那么重点就来了，`prototype` 是函数才会有的一个属性指向了一个对象 `Object`，这个对象正是调用该构造函数而创建的实例的原型而在我们的例子中就是 `person` 这个对象。
然后接着来看我们打印出来的这个对象中间有一个 `__proto__`，这个属性会指向该对象的原型。

```
function Person() {
}
var person = new Person();
console.log(person.__proto__ === Person.prototype); // true

```

我们再来看上面有一个`constructor`，那么这个又是什么呢？这个是由原型指向构造函数的一个属性。

```
function Person() {
}
console.log(Person === Person.prototype.constructor); // true
```

那么在实例和原型是是什么关系呢？

```
function Person() {
}
Person.prototype.name = 'tom'
var person = new Person()
person.name = 'jarry'
console.log(person.name) // jarry

delete person.name;
console.log(person.name) // tom

```

所以这里首先会从实例化的对象 `person` 去找 `name`，发现有是jarry，然后删除这个之后就会往上到原型去找，顺着 `person.__proto__` ，也就是 `Person.prototype` 中去找发现是tom，总之会一直往上去找直到没有，就是`null`，这个寻找的过程就可以理解为原型链。

懵懵懂懂还是功力不够，只能以自己能看懂的来写这些东西。。。虽然好像也没有太弄明白这个

ps：最后有一个一些比上面实用的东西，就是对象的`prototype`上有一个`hasOwnProperty`这个方法来检测是否含有某个键值，之前看到过这个是不会去循环遍历对象的，所以一般都拿这个来遍历。。。这也许是这篇文章最大的收获了。。。
