第一种，`props` 和 `$emit`
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>VUE</title>
</head>
 
<body>
  <div id="app">
    <children :value="msg" :parents-count="childrenCount" @ischange="hasClick"></children>
  </div>
</body>
<script crossorigin="anonymous" integrity="sha384-8t+aLluUVnn5SPPG/NbeZCH6TWIvaXIm/gDbutRvtEeElzxxWaZN+G/ZIEdI/f+y" src="https://lib.baomitu.com/vue/2.6.10/vue.min.js"></script>
<script type="text/javascript">
  Vue.component('Children', {
    props: {
      value: {
        type: String,
        default: 'children'
      },
      parentsCount: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        count: 0
      }
    },
    methods: {
      changeValue () {
        this.count++
        this.$emit('ischange', this.count)
      }
    },
    template:`
    <div>
      <p>{{value}}</p>
      <button @click="changeValue">{{parentsCount}}click me</button>
    </div>
    `
  })
  new Vue({
    el: '#app',
    data () {
      return {
        msg: 'hello world',
        childrenCount: 0
      }
    },
    methods: {
      hasClick(e) {
        this.childrenCount = e
      }
    }
  })
</script>
</html>
```
