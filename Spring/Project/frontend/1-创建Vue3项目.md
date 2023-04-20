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