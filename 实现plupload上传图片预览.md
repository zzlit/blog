之前一直使用这个插件实现上传图片，但是有一个问题一直没有解决，就是上传图片完成之后的预览问题，原先的做法是先上传至服务器，然后再从服务器拿图片来预览，这两天终于把这个功能改善了，实现了先预览再上传至服务器，其中还是踩了几个坑记录下来留给有需要的小伙伴使用。

首先这个插件的 [API](https://github.com/moxiecode/plupload/wiki/API) ，另外一个解决预览功能的另外（叫插件？还是说这个自带的功能）的 [Moxie API](https://github.com/moxiecode/moxie/wiki/API) ，其中 `plupload` 为我们提供了 `moxie` 这个对象，初始化的时候打印一下能看到，如果没有的话就需要自己引入。
我是 `npm` 安装的 `plupload` 首先需要引入：

```
import plupload from 'plupload'
```

然后初始化，事件监听什么的文档或者网上搜一下基本都有，我觉得能够看到这个文章的小伙伴这些都不是问题，主要还是来看看预览图片是怎么实现的，我主要是在 `filesAdded` 事件做的处理，先来看代码：


```
filesAdded (up, files) {
  const moxie = plupload.moxie
  const file = files[0]

  if (file.type === 'image/gif') {
    let fr = new moxie.file.FileReader()
    fr.onload = () => {
      this.imgurl = fr.result
      fr.destroy()
      fr = null
    }
    fr.readAsDataURL(file.getSource())
  } else {
    let preview = new moxie.image.Image()
    preview.onload = () => {
      preview.resize({
        width: 100,
        fit: false
      })
      preview.crop(100, 100)
      this.imgurl = preview.type === 'image/jpeg' ? preview.getAsDataURL('image/jpeg', 80) : preview.getAsDataURL()
      preview.destroy()
      preview = null
    }
    preview.load(file.getSource())
  }
}
```

这里我只限定了一张图片，所以 `files` 是个长度为1的数组，直接取第0项就是需要的文件对象了， `moxie` 是上面说的 `plupload` 自带的对象， `imgurl` 预览图片url。
我们先判断文件类型如果是gif就走 `new moxie.file.FileReader()` ，其他图片就 `new moxie.image.Image()` 这中间 `preview.resize` 和 `preview.crop` 是裁剪参数，可以注释掉。其他基本都是api上的内容，多看看我觉得就没什么问题了，我踩的主要的坑就是不知道还有 `moxie` 这个东西，官方的例子好像也没有表明出来，只是列出了上传文件列表的名字。

其实最主要还是自己的问题，不过实现了就很开心了，又算是做了一个小小的优化吧。

[整个代码地址](https://github.com/zzlit/imgCropper/blob/master/src/components/upload.vue)

附上一些参考的内容：

[plupload API](https://github.com/moxiecode/plupload/wiki/API)

[Moxie API](https://github.com/moxiecode/moxie/wiki/API)

[引入moxie报错](https://github.com/moxiecode/plupload/issues/1469#issuecomment-320438490)
