# Vue-Router

切换路由并不需要新的HTTP请求，可以减少刷新资源损耗

**情景案例：**在网上购买娃娃，挑选时进行一些设置（如颜色、大小等），若只用`js`实现，则`URL`不会改变，分享给朋友时，朋友点开的页面由于URL没变，获得的是刷新为默认情形的页面，无法获得设置好的颜色、大小信息。这时若用路由实现，URL后面会多一些内容，点开看到的是原页面资源重新加载后的内容而非默认页面。

## 基本操作

### 创建项目并引入Vue-router

正常进行：

```
npm init vue@latest
```

```
cd 'fileName'
npm install
```

安装`vue-router`依赖：

```
npm install vue-router@latest
```

在`main.js`中引入`vue-router`：

```javascript
//main.js
import './assets/main.css'

import { createApp } from 'vue'
import { createRouter, createWebHistory } from 'vue-router'
import App from './App.vue'
const router = createRouter({
    history: createWebHistory(),
    routes:[],
})

createApp(App).mount('#app')
```

其中`routes`数组中应该放对应的路由规则

### 配置createRouter

将`routes`里用到的组件全放在`src目录`下新建的文件夹`views`里

![](.\assets\VueRouter1.png)



然后引入组件，配完整个路由`createRouter()`函数：

注意在`createApp()`后面要加`use(router)`

```javascript
//main.js
import './assets/main.css'

import { createApp } from 'vue'
import { createRouter, createWebHistory } from 'vue-router'
import App from './App.vue'
import Home from './views/Home.vue'
import Child1 from './views/Child1.vue'
import Child2 from './views/Child2.vue'
const router = createRouter({
    history: createWebHistory(),
    routes:[
        {
            path: '/',
            component: Home
        },
        {
            path: '/child1',
            component: Child1
        },
        {
            path: '/child2',
            component: Child2
        }
    ],
})

createApp(App).use(router).mount('#app')

```

这样，就允许使用对应的路由规则了

### 将路由放进对应组件中

在这些路由组件应该放的父组件中加入`<vue-router>`标签，即可显示路由组件。通过调整`URL`可以切换这几个组件

```html
<!--App.vue-->
<template>
  <router-view></router-view>
</template>
```

这是`/`路由：

![](.\assets\VueRouter3.png)

这是`/child1`路由：

![](.\assets\VueRouter2.png)

可以在父组件中放入一些\<a>标签指向对应路由

```html
<!--App.vue-->
<template>
  <nav>
    <a href="/">Home</a>
    <a href="/child1">Child1</a>
    <a href="/child2">Child2</a>
  </nav>
  <router-view></router-view>
</template>
```

但是这样会在每次切换路由时重新渲染页面，这有时不是我们想要的

### 用\<router-link>标签实现路由跳转

```html
<!--app.vue-->
<template>
  <nav>
    <router-link to="/">Home</router-link>|
    <router-link to="/child1">Child1</router-link>|
    <router-link to="/child2">Child2</router-link>|
    <a href="/">Home</a>|
    <a href="/child1">Child1</a>|
    <a href="/child2">Child2</a>
  </nav>
  <router-view></router-view>
</template>
```

这样，前三个路由不会重新加载页面

### 用`router`文件夹简化`main.js`

目录结构：

![](.\assets\VueRouter4.png)

`index.js`文件配置：

```javascript
//router/index.js
import { createRouter, createWebHistory } from 'vue-router'

import Home from '../views/Home.vue'
import Child1 from '../views/Child1.vue'
import Child2 from '../views/Child2.vue'
const router = createRouter({
    history: createWebHistory(),
    routes:[
        {
            path: '/',
            component: Home
        },
        {
            path: '/child1',
            component: Child1
        },
        {
            path: '/child2',
            component: Child2
        }
    ],
})

export default router;
```

`main.js`文件配置：

```javascript
import './assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

createApp(App).use(router).mount('#app')
```

## 案例：复杂路由的设置

### 父组件中配置\<router-link>

+ **关键点1：**用`v-for`遍历，写成动态形式，而非一个一个手敲

```html
<!--App.vue-->
<template>
  <nav>
    <router-link to="/">主页</router-link>
    <router-link v-for="dataEgg in dataEggs" :key="dataEgg.id" :to="`/eggs/${dataEgg.type}`">{{ dataEgg.name }}</router-link>
  </nav>
  <router-view></router-view>
</template>
<script>
import dataEggs from './data.json'
export default {
  data(){
    return{
      dataEggs,
    }
  }
}
</script>
<style scoped>
</style>
```

### router/index.js中配置路由信息

+ **关键点2：**不要开重复的组件文件，而是在一个`.vue`文件中条件渲染
+ **关键点3：**配置路由时，用`:`实现动态路由组件

```javascript
//router/index.js
import { createRouter, createWebHistory } from 'vue-router'

import Home from '../views/Home.vue'
import Eggs from '../views/Eggs.vue'

import dataEggs from '../data.json'

const router = createRouter({
    history: createWebHistory(),
    routes:[
        {
            path: '/',
            component: Home
        },{
            path:'/eggs/:eggType',
            component:Eggs,
        }
    ],
})

export default router;
```

目录结构：

![](.\assets\VueRouter5.png)



### 根据 $route 对象中的值决定Eggs组件该渲染什么

先看一下`this.$route`对象中有什么：

```html
<!--Eggs.vue-->
<template>
    <h1>
        蛋-路由组件
    </h1>
</template>
<script>
import dataEggs from '../data.json'
export default {
    data(){
        console.log(this.$route);
    }
}
</script>
```

在打印的值中，

![](.\assets\VueRouter6.png)

注意params中的`eggType`字段，就是我们要的当前路由

于是：

```vue
<template>
    <h1>
        {{ dataEgg.name }}-路由组件
    </h1>
    <div class="">
        {{ dataEgg.description }}
    </div>
</template>
<script>
import dataEggs from '../data.json'
export default {
    computed:{
        eggType(){//返回当前路由
            return this.$route.params.eggType;
        },
        dataEgg(){//找到当前路由对应的数据
            return dataEggs.find(dataEgg=>dataEgg.type===this.eggType);
        }
    }
}
</script>
```

这样，就可以只用一个vue组件在页面上显示多个路由的内容

## 链接激活：对当前路由的标签设置样式

默认类为.router-link-active，当处于某一特定路由时，标签改变样式

```vue
<!--App.vue-->
<style scoped>
.router-link-active{
  color:red;
}
</style>
```

![](.\assets\VueRouter7.png)

### 类名修改

在createRouter的参数对象中，可加linkActiveClass属性，以调整链接激活的类名

```javascript
//router/index.js
const router = createRouter({
    history: createWebHashHistory(),
    routes:[
        {
            path: '/',
            component: Home
        },{
            path:'/eggs/:eggType',
            component:Eggs,
        }
    ],
    linkActiveClass:'egg-active',
})
```

这样，

```vue
<!--App.vue-->
<style scoped>
.egg-active{
  color:red;
}
</style>
```

## `CreateWebHistory()`和`CreateWebHashHistory()`的区别

（暂时没懂，先记录）

+ `CreateWebHistory()`：

HTML5模式，如果用户直接访问路由会出现错误，必须先访问根页面（只需加一个回退路由，即第一次进入时必须进根页面）

+ `CreateWebHashHistory()`：

它在内部传递的实际 URL 之前使用了一个哈希字符（`#`）。由于这部分 URL 从未被发送到服务器，所以它不需要在服务器层面上进行任何特殊处理。不过，**它在 SEO 中确实有不好的影响**。

## 重定向

“强行”将某特定路由定向到另一条路由

```javascript
//router/index.js
const router = createRouter({
    history: createWebHashHistory(),
    routes:[
        {
            path: '/',
            component: Home
        },{
            path:'/eggs/:eggType',
            component:Eggs,
        },{
            path:'/eggs',
            redirect:'/eggs/chicken-egg'
        }
    ],
    linkActiveClass:'egg-active',
})
```

这样，如果输入url为`/eggs`，会直接跳转到`/eggs/chicken-egg`

## 404-非法路由

两种实现方法：

1. 在`Eggs.vue`组件中，判定当前路由是否和`data.json`中的某个匹配

```vue
<!--Eggs.vue-->
<template>
    <div v-if="!dataEgg">
        404
    </div>
    <div v-else>
        <h1>
            {{ dataEgg.name }}-路由组件
        </h1>
        <div class="">
            {{ dataEgg.description }}
        </div>
    </div>
</template>
```

2. 在路由配置文件中，直接定义非法路由，并写一个组件用来装非法路由的显示

# ##########待施工##########

```javascript
//router/index.js
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router'

import Home from '../views/Home.vue'
import Eggs from '../views/Eggs.vue'
import NotFound from '../views/NotFound.vue'

import dataEggs from '../data.json'

const router = createRouter({
    history: createWebHistory(),
    routes: [
        {
            path: '/',
            component: Home
        }, {
            path: '/eggs/:eggType',
            component: Eggs,
        }, {
            path: '/:pathMatch(.*)*',
            component: NotFound,
        }
    ],
    linkActiveClass: 'egg-active',
})

export default router;
```

NotFound.vue写的是404时的组件

暂时没成功，不知道为什么

## 懒加载

在component后用()=>{import}箭头函数代替

```javascript
//router/index.js
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router'
import Home from '../views/Home.vue'

import dataEggs from '../data.json'

const router = createRouter({
    history: createWebHistory(),
    routes: [
        {
            path: '/',
            component: Home
        }, {
            path: '/eggs/:eggType',
            component: () => import('../views/Eggs.vue'),
        }
    ],
    linkActiveClass: 'egg-active',
})

export default router;
```

## 路由命名

可以更灵活的设置to的值进行路由跳转，`App.vue`和`router/index.js`的配置如下：

```vue
<!--App.vue-->
<template>
  <nav>
    <router-link to="/">主页</router-link>
    <router-link v-for="dataEgg in dataEggs"  :to="{name:'eggs',params:{eggType:dataEgg.type}}">{{ dataEgg.name }}</router-link>
  </nav>
  <router-view></router-view>
</template>
```

```javascript
//router/index.js
const router = createRouter({
    history: createWebHistory(),
    routes: [
        {
            path: '/',
            component: ()=>import('../views/Home.vue'),
        }, {
            path: '/eggs/:eggType',
            name: 'eggs',
            component: () => import('../views/Eggs.vue'),
        }
    ],
    linkActiveClass: 'egg-active',
})
```

