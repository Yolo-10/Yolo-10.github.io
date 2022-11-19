## 项目目录
```
—— build：项目构建(webpack)相关代码
—— config：配置目录，包括端口号等。我们初学可以使用默认的。
—— node_modules：npm加载的项目依赖模块
—— src：开发的目录，基本上要做的事情都在这个目录里
    |———— assets: 放置一些图片，如logo等
    |———— components: 目录里面放了一个组件文件，可以不用。
    |———— App.vue: 项目入口文件，我们也可以直接将组件写这里，而不使用 components 目录。
    |———— main.js: 项目的核心文件。
—— static	静态资源目录，如图片、字体等。
—— test	初始测试目录，可删除
—— .xxxx文件	这些是一些配置文件，包括语法配置，git配置等。
—— index.html	首页入口文件，你可以添加一些 meta 信息或统计代码啥的。
—— package.json	项目配置文件。
—— README.md	项目的说明文档，markdown 格式
```
## App.vue
```html
<!-- 展示模板 -->
<template>
  <div id="app">
    <img decoding="async" src="./assets/logo.png">
    <hello></hello>
  </div>
</template>
 
<script>
// 导入组件
import Hello from './components/Hello'
 
export default {
  name: 'app',
  components: {
    Hello
  }
}
</script>
<!-- 样式代码 -->
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

## id,属性,方法
```html
<div id="vue_det">
    <h1>site : {{site}}</h1>
    <h1>url : {{url}}</h1>
    <h1>{{details()}}</h1>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#vue_det', //el ------> id
        data: {  //data ------> 属性
            site: "菜鸟教程",
            url: "www.runoob.com",
            alexa: "10000"
        },//methods ------> 方法
        methods: {
            details: function() {
                return  this.site + " - 学的不仅是技术，更是梦想！";
            }
        }
    })
</script>
```