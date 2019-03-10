## 1.一段代码
```javascript
<script src="http://how2j.cn/study/vue.min.js"></script>
<div id="div1">{{jugg.name}}</div>
<script>
    var jugg = {"name":"jugg","hp":900}
    new Vue({
        el:'#div1',
        data:{
            message:jugg
        }
    })
</script>
```

## 2.常见标准vue
vue格式



```js
new Vue({
    el:'#div1',
    data:{json:"json"},
    methods:{
        fangfaming:function(){
            this.json="???";
        }
    }
})


```

### 3.监听事件：v-on:click=""
v-on:click="fangfaming";

@click="fangfaming";

`
<button @click="fangfaming">click</div>
`
### 4.条件语句v-if=""
```html
<div v-if="true">something</div>
<div v-else>other</div>
```

### 5.循环语句v-for=""

```html
<script src="http://how2j.cn/study/vue.min.js"></script>
<div id="div1">
    <div v-for="hero in heros">{{hero.hp}}</div>
</div>

<script>
    var data=[
        {name:"jugg",hp:"900"},
        {name:"am",hp:"800"},
        {name:"spe",hp:"1000"}
    ];
    new Vue({
        el:'#div1',
        data:{
            heros:data
        }
    })


</script>
```

### 6.属性单向绑定v-bind:href=""

```HTML
<script src="http://how2j.cn/study/vue.min.js"></script>
<div id="div1">
    <a v-bind:href="link">come click</a>
</div>
<script>
new Vue({
    el:'#div1',
    data:{
        link:"http://www.4399.com"
    }
})
</script>
```


### 7.属性双向绑定v-model=""

- v-model.lazy=""  等输入完毕，再进行绑定
- v-model.number=""  转换类型为number
- v-model.trim=""  去掉前后的空格

```html
<script src="http://how2j.cn/study/vue.min.js"></script>
<div id="div1">
    <input type="text" v-model="input">
    <p>{{input}}</p>
</div>
<script>
new Vue({
    el:'#div1',
    data:{
        input:"http://www.4399.com"
    }
})
</script>
```

### 8.计算属性computed:{}

```js
new Vue({
      el: '#div1',
      data: {
          exchange:6.4,
          rmb:0
      },
      computed:{
          dollar:function() {
              return this.rmb / this.exchange;
          }
      }
    })
```

### 9.监听属性watch:{}

### 10.过滤器filters:{}或者Vue.filter({})

### 11.组件components:{'product':{template:''}}或者Vue.component('',{})

### 12.自定义函数x-art

### 13.路由(局部刷新)

### 14.ajax-axios

引入axios.js

从restful api获取json数据

```js
<script>
    var url = "http://how2j.cn/study/json.txt"
    axios.get(url).then(function(response){
        var json = response.data;
    })

</script>
```
