---
title: For-Vue
date: 2020-12-20 21:05:40
tags: Vue
category: 前端
# excerpt: 学习前端使我快乐 # 摘要
index_img: /img/maojingzhang.jpg
banner_img: /img/oumeixiaozhen.jpg
---
### VUE、Elementui、Code的创建和使用

npm install 不然报错 web package dev server

1、打开vscode的终端ctrl+`;
2、安装全局vue-cli客户端：npm install -g vue-cli
3、创建vue项目：vue init webpack （projectName项目名字）   运行vue项目：npm run dev 需要切进项目中 cd Coffee-vue
4、安装ElementUI：npm i element-ui -S
5、引入至项目中：![1585727261448](1585727261448.png)

6、取消报错的红线
在项目中build（编译）文件夹下webpack.base.conf.js文件中注释掉eslint的内容![1585728290551](1585728290551.png)

7、![1585800861965](1585800861965.png)

需要将其注释掉不然会在页面中有其效果
8、将其初始化路径更改为我们需要的路由![1585800926956](1585800926956.png)

9、
点击图片并改变图片的路径：简单步骤(vue的路径使用需要加:例如 :src)

```vue
<img class="menu" v-on:click= "flag=!flag" :src="flag?menupic:crosspic">

export default{
  data () {
    return {
      flag: true,
      menupic: require('../picture/menu.png'),
      crosspic: require('../picture/Cross.png'),
      logopic: require('../picture/coffeelogo.png')
    }
  },
  methods: {
  }
}
```

10、注意进入cd Coffee-vue必须要和文件名保持一致![1585812586935](1585812586935.png)

不然会报错
11、
走马灯图片设置 图片使用外链加载 

```vue
<!--卡片走马灯-->
        <el-carousel :interval="4000" type="card" height="350px">
          <el-carousel-item v-for="item in listpic" :key="item" width="100%">
            <img :src='item' width="100%">
          </el-carousel-item>
        </el-carousel>
<!--vue数据-->
data () {
    return {
      flag: true,
      showldiv: true,
      showmain: true,
      menupic: require('../picture/Menu.png'),
      crosspic: require('../picture/Cross.png'),
      logopic: require('../picture/coffeelogo.png'),
      coffeeface: require('../picture/Coffee-Face.jpg'),
      listpic: [('https://i1.fuimg.com/715304/395e39fc6dd0330a.png',
         'https://i1.fuimg.com/715304/a598958f4083fc17.jpg',
         'https://i1.fuimg.com/715304/40405d692956cfea.jpg',
         'https://i2.tiimg.com/715304/3d05a666162c5531.jpg',
         'https://i2.tiimg.com/715304/46ca868eb0fa445b.jpg',
         'https://i1.fuimg.com/715304/958f42f82a306944.jpg')
      ]
    }
  },
```

12、npm run build编译后的提示 需要将文件夹直接放置在服务器上 不影响

```vue
Tip: built files are meant to be served over an HTTP server.
  Opening index.html over file:// won't work.
```

13、 main.js的内容

```vue
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

Vue.config.productionTip = false
Vue.use(ElementUI)
/* eslint-disable no-new */
new Vue({
  // el是index.html的div id
  el: '#App',
  router,
  // App模块是App.vue
  components: { App },
  template: '<App/>'
})
```

index.html的内容为空

14、页面刷新的使用(在App页面中做定义)

```vue
<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <router-view v-if="isRouterAlive"/><!--这个是必须要有的跳转的路由-->
  </div>
</template>

<script>
export default {
  name: 'App',
  provide () {
    return {
      reload: this.reload // 刷新定义
    }
  },
  data () {
    return {
      isRouterAlive: true
    }
  },
  methods: {
    reload () {
      this.isRouterAlive = false
      this.$nextTick(function () {
        this.isRouterAlive = true
      })
    }
  }
}
</script>

<script>
//页面使用
export default({
  inject: ['reload'], // reload的使用 就好像继承一样
  name: 'CommModity',
  data: function () {
    return {
      tableData: [],
      Options: [],
      SelectValue: '',
      needLoadingRequestCount: 0
    }
  },
    methods: {
        handleDelete (index, row) {
          var that = this
          // elementui confirm 提示
          this.$confirm('此操作将永久删除此信息, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            that.$axios({
              method: 'GET',
              url: that.api.baseURL + 'Commodity/DeleteComm?CommId=' + row.id
            }).then(() => { // 这里使用了ES6的语法
              that.$message({
                type: 'warning',
                message: '删除成功'
              })
              that.reload()
            }).catch((error) =>
              console.log(error) // 请求失败返回的数据
            )
          })
        }
      },
    }
}
  
</script>
```

15、局部Loading

```vue
// 打开loading
let loadingInstance = Loading.service({
        lock: true,
        text: 'Loading...',
        background: 'rgba(0,0,0,0.1)',
        target: document.querySelector('#app div')
      })
loadingInstance.close() // 关闭loading
```

16、Vue-ElementUI-Admin框架的下载安装

git：https://github.com/PanJiaChen/vue-element-admin.git

下载node-sass：

npm config set sass_binary_site=https://npm.taobao.org/mirrors/node-sass
重新 npm install

ssh设置：$ ssh-keygen -t rsa -C '2545664565@qq.com'

npm run dev