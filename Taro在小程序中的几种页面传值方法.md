  19年的第一篇博客，有很多打算，但是都是藏在心里，只有迈出了第一步才能算付诸实践，那么就从博客开始吧，希望自己能坚持下去，加油！
  这一篇主要讲讲在taro中针对小程序的几种页面传参方法吧，也有一定的预加载的意味，不仅仅是在小程序中，在网页上预加载这个都是很有必要而且有意义的，至于何为预加载，意义又是什么这里就不多做说明了，直接进入正题吧。
  首先的两种方法，来自于taro官网[预加载一篇](https://nervjs.github.io/taro/docs/best-practice.html#%E9%A2%84%E5%8A%A0%E8%BD%BD)
  >Taro 提供了 componentWillPreload 钩子，它接收页面跳转的参数作为参数。
  它在`componentWillMount `之前执行，这里有个小坑：如果你的小程序有分包的话这个只能在同一个分包下能将页面跳转的参数带过来，在不同的分包下因为第一次没有下载，所以就会导致第一次没有打印，也就是说第一次并没有走`componentWillPreload`这个生命周期，它的使用也很简单
  ```
    componentWillPreload (params) {
    return this.func(params.id)
  }
  ```
  `params`里面即带有你传过来的参数，可以把你预加载的内容在它`return`返回，这里有个限制就是跳转的路径必须是**绝对路径**，相对路径并不会触发这个钩子函数，同时官网也有说
  >可以把需要预加载的内容通过 return 返回，然后在页面触发 componentWillMount 后即可通过 this.$preloadData 获取到预加载的内容。
  但是我有尝试过打印这个`this.$preloadData`里面的值是`undefined`，所以我就一般在`componentWillPreload`这个函数返回那里直接调用获取数据的接口了。
  那么如果想在不同的分包里面使用预加载该怎么用呢？这里就来到了第二种方法，使用`this.$preload`跳转传参。
  `this.$preload`也是小程序独有的，它的使用情况是当你不喜欢在你的url后面携带太多参数，那么就可以使用它来传参，我们拿官网的例子来举例
  ```
  // 传入多个参数
  // A 页面
  this.$preload({
    x: 1,
    y: 2
  })
  Taro.navigateTo({ url: '/pages/B/B' })

  // B 页面
  componentWillMount () {
    console.log('preload: ', this.$router.preload)
  }
  ```
  它的触发在`componentWillMount`这个生命周期里面，这里就可以应对上面那种情况，当分包的情况下从当前包跳转到下一个包去。
  使用的方法`this.$preload(key:String|Object, [value: Any])`
  那么这里第三中方法则是我在vue中使用过的一种方法`this.$router.params`，在小程序里面也能使用，页面的带参跳转就相当于是路由带参跳转，它使用的生命周期是在`componentWillMount`里面。
  后面的方法一种是使用`redux`传值，一种是使用全局变量`setGlobalData``getGlobalData `，或者使用`Taro.setStorageSync('key', value)`存在本地缓存中，然后使用`Taro.getStorageSync('key')`来取就行了。虽然这里已经算不上是页面的传参了，但是我个人觉得还是有一点参考价值的。
