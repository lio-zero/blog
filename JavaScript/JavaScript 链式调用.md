# JavaScript 链式调用

如果你使用过 JQuery，你应该会经常看到这样的操作：

> `$('text').css('color', 'pink').show()` —— 这就是链式调用。

也就是在调用一个方法之后，还可以在该方法后面继续 `.xxx()` 调用其他方法，呈现链条式：

```js
_([1, 2, 3])
  .map((n) => n * n)
  .tap(console.log)
  .thru((n) => n.reverse())
  .value()
```

以上示例是使用 Lodash 库处理一个数组所调用的一些方法，跟 jQuery 一样都支持链式调用。

它也被称为[流式接口](https://zh.wikipedia.org/wiki/%E6%B5%81%E5%BC%8F%E6%8E%A5%E5%8F%A3)（fluent interface），但开发者们更愿意称它为方法链式调用。

下面我们来看看如何实现链式调用。

## 函数调用

在实现链式调用之前，我们先来看看一般函数的调用方式：

```js
function F(name) {
  this.name = name
  this.sayHi = function () {
    console.log(this.name)
  }
  this.modifyName = function (name) {
    this.name = name
  }
}

const f = new F('IU')

f.sayHi() // IU
f.modifyName('UI')
f.sayHi() // UI
```

可以看到，我们要调用函数，需要多次重复使用一个对象变量。链式调用可以简化这样的操作。

## 链式调用

链式调用实现很简单，我们只需要在构造函数/类中创建方法时，`return this` 返回当前调用方法的对象，可以实现链式调用方法。

```js
function Person(name) {
  this.name = name

  this.sayHi = function () {
    console.log(this.name)
    return this
  }

  this.modifyName = function (name) {
    this.name = name
    return this
  }
}

var person = new Person('IU')

person.sayHi().modifyName('UI').sayHi()
// IU
// UI
// Person { name: 'UI', sayHi: f, modifyName: f }
```

通过使用链式调用，我们最终可以得到更简洁、更易于理解的代码。

另外，我们可以编写一个函数来自动添加 `return this`：

```js
function chain(obj) {
  Object.keys(obj).forEach(function (key) {
    var member = obj[key]
    if (typeof member === 'function' && !/\breturn\b/.test(member)) {
      obj[key] = function () {
        member.apply(this, arguments)
        return this
      }
    }
  })
}
```

我们需要稍微改造一下示例，将方法添加的构造函数原型上：

```js
Person.prototype.sayHi = function (name) {
  console.log(this.name)
}

Person.prototype.modifyName = function (name) {
  this.name = name
}

chain(Person.prototype)
```

尝试运行它，你会得到相同的结果。
