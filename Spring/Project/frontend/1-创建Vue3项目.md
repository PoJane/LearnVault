- Vue3官网：[https://cn.vuejs.org/](https://cn.vuejs.org/)
- Axios文档：[https://www.axios-http.cn/docs/intro](https://www.axios-http.cn/docs/intro)
- ElementPuls官网: [https://element-plus.gitee.io/zh-CN/guide/quickstart.html](https://element-plus.gitee.io/zh-CN/guide/quickstart.html)
前端项目使用Vue框架，使用Vue-Axios和Axios与后端进行跨域数据交互。项目依赖于node.js环境，需要先安装配置node.js。

### 1. 创建Vue3模板项目
##### 1.1 npm安装最新Vue
1. cd到你想创建项目的目录，使用如下命令
```
npm init vue@latest
```
写好项目名称project_frontend后按enter键默认不添加模块，也可以添加。

2. 安装项目
初始化完成后，按照node给的提示继续进行。
```
D:\MyWeb\MyProject>cd project_frontend

D:\MyWeb\MyProject\project_frontend>npm install
npm WARN deprecated sourcemap-codec@1.4.8: Please use @jridgewell/sourcemap-codec instead

added 32 packages in 13s
```

3. 运行项目
根据提示运行如下命令
```
D:\MyWeb\MyProject\project_frontend>npm run dev

> project_frontend@0.0.0 dev
> vite


  VITE v4.2.2  ready in 696 ms

  ➜  Local:   http://127.0.0.1:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help
```
若成功，即可到http://127.0.0.1:5173/看见默认Vue3页面。

4. 用vscode或其他ide打开项目。

### 2. 数据交互
##### 2.1 安装Axios
可先到到Axios官网熟悉axios
1. 安装axios和vue-axios
```
npm install axios
npm install vue-axios
```

2. 在项目中全局引用
- 在main.js中导入vue-axios和axios，并作出适当修改。
```js
import { createApp } from 'vue'
import App from './App.vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

import './assets/main.css'

const app=createApp(App)
app.use(VueAxios,axios)
app.mount('#app')
```
- 注意，use函数中VueAxios要放在axios前，否则会出现Uncaught RangeError: Maximum call stack size exceeded错误。

##### 2.2 使用axios与后端数据交互
1. 官网描述
- 官网给出的get方法：
```js
axios.get('/user?ID=12345') 
	.then(function (response) {
	// 处理成功情况 
	console.log(response); 
	}) .catch(function (error) { 
	// 处理错误情况 
	console.log(error);
	}) .then(function () { 
	// 总是会执行 
});
```
大致按照如上结构编写get方法，获得后端输出的数据。

2. 使用axios
将App.vue中的内容删除到只剩script和template元素。
- 在script中定义组件和数据。
```vue
<script>
  export default{
    data(){
      return{
        words:{
          id:0,
          data:""
        }
      }
    }
  }
</script>
```
- 在methods中创建函数，函数中调用axios.get方法，最后在mounted中调用该函数。
```vue
<script>
  export default{
    data(){
      return{
        words:{
          id:0,
          data:""
        }
      }
    },
    methods:{
      showWords(){
        var _this=this;
        _this.axios.get('http://localhost:8081')
        .then(function(response){
          console.log(response)
        }).catch(function(error){
          console.log(error)
        }).then(function(){
          console.log("axios done")
        })
      }
    },
    mounted(){
      this.showWords()
    }
  }

</script>
```

3. 跨域访问
由于前后端地址是不同的，需要在后端设置，以允许前端进行跨域访问。
详见[[3-跨域访问设置]]

4. 运行
- 运行idea中运行后端项目，然后使用`npm run dev`运行前端vue项目，可以在浏览器-检查-控制台中看到返回的数据，因为没有向后端传递参数，此时接收的是error。
![[Pasted image 20230420130013.png]]

- 传参
将get中的url改为'http://localhost:8081?id=1'
即可看到正确的‘Hello! World!’字符串
![[Pasted image 20230420130236.png]]

##### 2.3 将数据写到前端页面
axios将成功获得的数据包装为response，response中的data才是后端真正传回的数据，可以使用response.data访问。
- 将response.data赋值给前端定义的words
```vue
<script>
  export default{
    data(){
      return{
        words:{
          id:0,
          data:""
        }
      }
    },
    methods:{
      showWords(){
        var _this=this;
        _this.axios.get('http://localhost:8081?id=1')
        .then(function(response){
          console.log(response)
          _this.words=response.data
        }).catch(function(error){
          console.log(error)
        }).then(function(){
          console.log("axios done")
        })
      }
    },
    mounted(){
      this.showWords()
    }
  }
</script>

<template>
  <h1>{{ words.data }}</h1>
</template>
```

- 浏览器即可显示数据
![[Pasted image 20230420131249.png]]