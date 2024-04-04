# `Vue Basics`

## 快速启动

命令行中输入：

```
npm create vue@latest
```

即可创建一个vue脚手架

### 创建一个`vue`应用

每个 `Vue` 应用都是通过`createApp`函数创建一个新的**应用实例**

其中，传入`createApp`的对象是一个组件，返回值是一个应用

```javascript
import { createApp } from 'vue'
// 从一个单文件组件中导入根组件
import App from './App.vue'

const app = createApp(App)
```

### 挂载应用

应用实例必须在调用了 `.mount()` 方法后才会渲染出来。该方法接收一个“容器”参数，可以是一个实际的 DOM 元素或是一个 CSS 选择器字符串

即：需要把上面创建的那个vue应用“镶嵌”到一个容器中

```html
<div id="app"></div>
```

```javascript
app.mount('#app')
```

应用根组件的内容将会被渲染在容器元素里面。容器元素自己将**不会**被视为应用的一部分。

`.mount()` 方法应该始终在整个应用配置和资源注册完成后被调用。同时请注意，不同于其他资源注册方法，它的返回值是根组件实例而非应用实例。

### 应用配置（暂时没懂）

应用实例会暴露一个 `.config` 对象允许我们配置一些应用级的选项，例如定义一个应用级的错误处理器，用来捕获所有子组件上的错误：

```javascript
app.config.errorHandler = (err) => {
  /* 处理错误 */
}
```

应用实例还提供了一些方法来注册应用范围内可用的资源，例如注册一个组件：

```javascript
app.component('TodoDeleteButton', TodoDeleteButton)
```

这使得 `TodoDeleteButton` 在应用的任何地方都是可用的。我们会在指南的后续章节中讨论关于组件和其他资源的注册。你也可以在 [API 参考](https://cn.vuejs.org/api/application.html)中浏览应用实例 API 的完整列表。

确保在挂载应用实例之前完成所有应用配置！

### 多个应用实例

应用实例并不只限于一个。`createApp` API 允许你在同一个页面中创建多个共存的 Vue 应用，而且每个应用都拥有自己的用于配置和全局资源的作用域。

```javascript
const app1 = createApp({
  /* ... */
})
app1.mount('#container-1')

const app2 = createApp({
  /* ... */
})
app2.mount('#container-2')
```

如果你正在使用 Vue 来增强服务端渲染 HTML，并且只想要 Vue 去控制一个大型页面中特殊的一小部分，应避免将一个单独的 Vue 应用实例挂载到整个页面上，而是应该创建多个小的应用实例，将它们分别挂载到所需的元素上去。

## 目录结构

```
.vscode: vscode工具的配置文件夹

node_modules: 支持项目运行的依赖文件

public: 资源文件夹

src: 源码文件夹

.gitignore: git忽略文件

index.html: 入口html文件

package.json: 信息描述文件

README.md: 注释文件

vite.config.js: Vue配置文件
```

## 模板语法：

### 最基本：模板插值

```vue
<template>
<h1>标题</h1>
<p>{{ msg }}</p> 
</template>
<script>
export default{
    data(){
        return {
            msg:"神奇的语法"
        }
    }
}
</script>
```

### 使用JavaScript表达式

```vue
<template>
<h1>标题</h1>
<p>{{ msg }}</p> 
<p>{{ number + 1}}</p>
<p>{{check?"Yes":"No"}}</p>
</template>
<script>
export default{
    data(){
        return {
            msg:"神奇的语法",
            number: 10,
            check: true
        }
    }
}
</script>
```

注：必须是一个表达式，不能是一个非表达式的语句。如

```
{{var a=1; }}
{{if(check){ return message}}}
```

等语句都不行

### 原始HTML

```vue
<template>
<h1>标题</h1>
<p>{{ msg }}</p> 
<p>{{ number + 1}}</p>
<p>{{check?"Yes":"No"}}</p>
<p v-html="raw-html"></p>
</template>
<script>
export default{
    data(){
        return {
            msg:"神奇的语法",
            number: 10,
            check: true,
            raw-html:"<a href='https://www.baidu.com'>百度</a>"
        }
    }
}
</script>
```

## 属性绑定

### 单个属性绑定 v-bind:或单个:

将data中的值作为类名、id名等：需要加v-bind

```vue
<template>
  <div v-bind:class="msg" v-bind:id="appId">测试</div>
</template>

<script>
export default{
  data(){
    return {
      msg: "active",
      dynamicId:"appId"
    }
  }
}
</script>

```

如果对象中某值为null或undefined，则不会出现（下例中title属性不会出现）

```vue
<template>
  <div v-bind:class="msg" v-bind:id="dynamicId" v-bind:title="dynamicTitle">测试</div>
</template>

<script>
export default{
  data(){
    return {
      msg: "active",
      dynamicId:"appId",
      dynamicTitle:null,
    }
  }
}
</script>
```

v-bind:等价于:（一个冒号）

```vue
<template>
  <div :class="msg" :id="dynamicId" v-bind:title="dynamicTitle">测试</div>
</template>

<script>
export default{
  data(){
    return {
      msg: "active",
      dynamicId:"appId",
      dynamicTitle:null,
    }
  }
}
</script>
```

### 一次绑定多个值

```vue
<template>
  <div v-bind="objectOfAttrs">测试</div>
</template>

<script>
export default{
  data(){
    return {
      objectOfAttrs:{
        id: appId,
        class: active,
      },
    }
  }
}
</script>
```

这里自动绑定了id="appId"和class="ative"

### \`${ }\` （模版字符串）

```html
<template>
  <div :class="`${divStyle}-container`">测试</div>
</template>
<script>
export default{
  data(){
    return {
        divStyle:"test",
    }
  }
}
</script>
<style scoped>
.test-container{
  color:red;
}
</style>
```

`{}`里面放data内的变量，而其他地方为普通字符串

上述例子中，”测试“会变成红色

## 条件渲染

### v-if="", v-else, v-else-if=""

若引号内值为true,则显示，否则不显示；注意引号内可以是表达式；可以配合v-else、v-else-if使用

```vue
<template>
  <h1>条件渲染</h1>
  <div v-if="flag">你能看见我吗</div>
  <div v-else>那你还是看看我吧</div>
  <div v-if="type==='A'">A</div>
  <div v-if="type==='B'">B</div>
  <div v-if="type==='C'">C</div>
</template>

<script>
export default{
    data(){
        return{
            flag:false,
            type:'A',
        }
    }
}
</script><template>
  <h1>条件渲染</h1>
  <div v-if="flag">你能看见我吗</div>
</template>

<script>
export default{
    data(){
        return{
            flag:true,
        }
    }
}
```

### v-show=""

当引号内表达式为true时，显示；否则不显示

```vue
<template>
  <h1>条件渲染</h1>
  <div v-show="!flag">show</div>
</template>

<script>
export default{
    data(){
        return{
            flag:false,
        }
    }
}
</script>

```



### v-show和v-if的区别

v-if：如果在初次渲染时为false，则直接什么事都不干；为true时渲染。如果多次切换true/false，每次变成true都会重新渲染一次

v-show：基于css的display: none，初始时直接渲染出来，根据布尔值决定是否显示出来

**如果频繁切换，优先使用v-show；如果切换少，优先使用v-if**

## 列表渲染

### v-for=""

#### 遍历数组

```vue
<template>
    <h3>列表渲染</h3>
    <div v-for="item in names">{{ item }}</div>
</template>
<script>
export default{
    data(){
        return {
            names:["111","222","333"],
        }
    }
}
</script>
```

上述代码含义：遍历数组中每一个元素，打印它本身

注：上述for循环中in也可以替换为of

#### 数组遍历中，使用(item, index): 分别对应对象、索引值

```vue
<template>
  <h3>列表渲染</h3>
  <div v-for="(item, index) in names">{{ item }}:{{ index }}</div>
</template>
<script>
export default {
  data() {
    return {
      names: ["111", "222", "333"],

    };
  },
};
</script>
```

#### 遍历对象：

默认遍历打印的是对象的值

```vue
<template>
  <h3>列表渲染</h3>
  <div v-for="item in type">{{ item }}</div>
</template>
<script>
export default {
  data() {
    return {
      names: ["111", "222", "333"],
      type:{
        id:1,
        name:"Bob",
        content:"12345",
      }
    };
  },
};
</script>
```

#### 三个属性：(item, key, index)分别对应值、键、索引

```vue
<template>
  <h3>列表渲染</h3>
  <div v-for="(item,key,index) in type">{{ item }}:{{ key }}:{{ index }}</div>
</template>
<script>
export default {
  data() {
    return {
      names: ["111", "222", "333"],
      type:{
        id:1,
        name:"Bob",
        content:"12345",
      }
    };
  },
};
</script>
```

渲染结果：

```
1:id:0
Bob:name:1
12345:content:2
```

### 通过key管理状态

在for后面加 :key=""，那么

不增加该属性：每次当data内容更新时，会再次渲染整个列表；若增加这一行，之前已有的不会重复渲染。

实例略

## 事件处理

### 内联事件处理器v-on:click=""或@click=""

```vue
<template>
  <h3>内联事件处理器</h3>
  <button v-on:click="count++">add</button>
  <p>{{ count }}</p>
</template>
<script>
export default {
  data() {
    return {
      count: 0,
    };
  },
};
</script>
```

上述代码：count初始化为0；每次按button，count+1

### 方法事件处理器：

将函数打包至methods:{}下

```vue
<template>
    <h3>内联事件处理器</h3>
    <button @click="addCount">add</button>
    <p>{{ count }}</p>
  </template>
  <script>
  export default {
    data() {
      return {
        count: 0,
      };
    },
    methods:{
        addCount(){
            this.count++;
            console.log(count);
        }
    }
  };
  </script>
```



## 事件传参

### Vue中的event对象就是原生JavaScript中的event对象

```vue
<template>
  <h3>内联事件处理器</h3>
  <button @click="addCount">add</button>
  <p>{{ count }}</p>
</template>
  <script>
export default {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    addCount(e) {
      //Vue中的event对象就是原生Js中的event对象
      e.target.innerHTML = "ADD" + this.count;//注：e.target表示调用这个函数的这个标签
      this.count++;
    },
  },
};
</script>
```

### 传递参数

易懂，不解释

```vue
<template>
  <h3>内联事件处理器</h3>
  <button v-on:click="addCount('Hello')">add</button>
  <p>{{ count }}</p>
</template>
  <script>
export default {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    addCount(msg) {
      console.log(msg);
      this.count++;
    },
  },
};
</script>
```

demo: 点击paragraph会在控制台上打印对应的值

```vue
<template>
  <h3>内联事件处理器</h3>
  <p v-for="item in name" @click="printName(item)">{{ item }}</p>
</template>
  <script>
export default {
  data() {
    return {
      count: 0,
      name:['a','b','c'],
    };
  },
  methods: {
    printName(name){
      console.log(name);
    },
  },
};
</script>
```

### 在@click中获取event对象的方法：$event

```vue
<template>
  <h3>内联事件处理器</h3>
  <p v-for="item in names" @click="printName(item,$event)">{{ item }}</p>
</template>
  <script>
export default {
  data() {
    return {
      count: 0,
      names: ["a", "b", "c"],
    };
  },
  methods: {
    printName(name,e) {
      console.log(name);
      console.log(e);
    },
  },
};
</script>
```

每次打印对应name值的同时还会打印对应的\<p>对象

## 事件修饰符



### 阻止默认事件

原生Js写法：e.preventDefault()

```vue
<template>
    <h3>事件修饰符</h3>
    <a v-on:click="clickHandle" href="https://www.baidu.com">百度</a>
</template>
<script>
export default{
    methods:{
        clickHandle(e){
            //阻止默认事件
            e.preventDefault();
            console.log("点击了");
        }
    }
}
</script>
```

Vue写法：

```vue
<template>
    <h3>事件修饰符</h3>
    <a v-on:click.prevent="clickHandle" href="https://www.baidu.com">百度</a>
</template>
<script>
export default{
    methods:{
        clickHandle(){
            console.log("点击了");
        }
    }
}
</script>
```

### 其他事件略，官方文档见下

[官方文档]([事件处理 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/event-handling.html#accessing-event-argument-in-inline-handlers))

## 侦测数组变更

**能够在数组发生变化时自动渲染的方法：**

push(), pop(), shift(), unshift(), splice(), sort(), reverse()

```vue
<template>
    <h3>监听数组变化</h3>
    <button @click="addListHandle">添加数据</button>
    <ul>
        <li v-for="name in names" :key="index">{{ name }}</li>
    </ul>
</template>
<script>
export default{
    data(){
        return{
            names:["ime","jack","bob"],
        }
    },
    methods:{
        addListHandle(){
            this.names.push("eric");
        }
    }
}
</script>
```

上述代码中，点击按钮后列表后面便会显示出添加的字符串

+ **不能在数组发生变化时自动渲染的方法：**

filter(), concat(), slice()

如果上述代码中的addListHandle()方法里改为

```vue
this.names.concat(["eric"]);
```

则不会渲染出新添加的字符串

数组发生变化时不自动渲染的解决方法：

```javascript
this.nums1=this.nums1.concat(this.nums2);
```

## 计算属性

在{{}}中虽然可以放表达式，但是有时这样写会显得过于复杂，不易维护。可以将这些表达式放入计算属性中

**例：下列代码**

```vue
<template>
    <h3>{{ itbaizhan.name }}</h3>
    <p>{{ itbaizhan.content.length>0?'Yes':'No' }}</p>
</template>
<script>
export default{
    data(){
        return{
            itbaizhan:{
                name:"百战程序员",
                content:["前端","java","python"],
            }
        }
    },
}
</script>
```

**可以改变为：**

```vue
<template>
    <h3>{{ itbaizhan.name }}</h3>
    <p>{{ itbaizhanContent }}</p>
</template>
<script>
export default{
    data(){
        return{
            itbaizhan:{
                name:"百战程序员",
                content:["前端","java","python"],
            }
        }
    },
    computed:{
        itbaizhanContent(){
            return this.itbaizhan.content.length>0?'Yes':'No';
        }
    },
}
</script>
```

### 计算属性缓存VS方法

计算属性的值会基于其响应式依赖被缓存，只要表达式的内容不变，多次调用不会重复计算；而方法每调用一次计算一次

## Class绑定

### 单个属性绑定：

可以用最常见的v-bind:或:来进行绑定，但不推荐，因为这样不宜修改

```vue
<template>
  <p :class="myClass">Class样式绑定</p>
</template>
<script>
export default {
  data() {
    return {
      myClass: "demo",
    };
  },
};
</script>
<style scoped>
.demo {
  color: red;
  font-size: large;
}
</style>
```

这里vue给class多了一些功能，可以用以下方式用对象写：

```vue
<template>
  <p :class="{'active':isActive}">Class样式绑定</p>
</template>
<script>
export default {
  data() {
    return {
      isActive:true,
    };
  },
};
</script>
<style scoped>
.active {
  color: red;
  font-size: large;
}

</style>
```

以上代码中，若isActive值为true，则显示active类的属性；若为false，不显示

### 使用对象进行多个class属性绑定：

```vue
<template>
  <p :class="{'active':isActive,'danger':isDanger}">Class样式绑定</p>
</template>
<script>
export default {
  data() {
    return {
      isActive:true,
      isDanger:true,
    };
  },
};
</script>
<style scoped>
.active {
  color: red;
  font-size: large;
}
.danger{
    text-decoration: line-through;
}
</style>
```

上述代码中，p标签可以有两种类型，通过调整isActive和isDanger的值调整

+ 也可以写作：

```vue
<template>
  <p :class="myClass">Class样式绑定</p>
</template>
<script>
export default {
  data() {
    return {
      myClass:{'active':true,'danger':true},
    };
  },
};
</script>
<style scoped>
.active {
  color: red;
  font-size: large;
}
.danger{
    text-decoration: line-through;
}
</style>
```

上述代码与前文代码效果相同

### 使用数组绑定多个对象

```vue
<template>
  <p :class="[arrActive,arrDanger]">Class样式绑定</p>
</template>
<script>
export default {
  data() {
    return {
      isActive:true,
      isDanger:true,
      myClass:{'active':true,'danger':true},
      arrActive:'active',
      arrDanger:'danger',
    };
  },
};
</script>
<style scoped>
.active {
  color: red;
  font-size: large;
}
.danger{
    text-decoration: line-through;
}
</style>
```

数组中可以用三元运算符：

```vue
<template>
  <p :class="[isActive?'active':'',isDanger?'danger':'']">Class样式绑定</p>
</template>
<script>
export default {
  data() {
    return {
      isActive:true,
      isDanger:true,
      myClass:{'active':true,'danger':true},
      arrActive:'active',
      arrDanger:'danger',
    };
  },
};
</script>
<style scoped>
.active {
  color: red;
  font-size: large;
}
.danger{
    text-decoration: line-through;
}
</style>
```

+ 数组中可以嵌套对象：

```vue
<template>
  <p :class="[isActive?'active':'',{'danger':isDanger}]">Class样式绑定</p>
</template>
<script>
export default {
  data() {
    return {
      isActive:true,
      isDanger:true,
      myClass:{'active':true,'danger':true},
      arrActive:'active',
      arrDanger:'danger',
    };
  },
};
</script>
<style scoped>
.active {
  color: red;
  font-size: large;
}
.danger{
    text-decoration: line-through;
}
</style>
```

## Style绑定

### 用对象绑定多个属性

```vue
<template>
    <p :style="{color:activeColor,fontSize:activeFontSize+'px'}">Style绑定1</p>
</template>
<script>
export default {
    data(){
        return{
            activeColor:'green',
            activeFontSize:40
        }
    }
}
</script>
```

等价于：

```vue
<template>
    <p :style="{color:activeColor,fontSize:activeFontSize+'px'}">Style绑定1</p>
    <p :style="styleObject">Style绑定2</p>
</template>
<script>
export default {
    data(){
        return{
            activeColor:'green',
            activeFontSize:40,
            styleObject:{
                color:"red",
                fontSize:'30px'
            },
        }
    }
}
</script>
```

## 侦听器

在export default中增加一个新对象watch，里面的函数名必须与被监听的数据名相同

```vue
<template>
  <h3>侦听器</h3>
  <p>{{ message }}</p>
  <button @click="updateHandle">修改数据</button>
</template>
<script>
export default {
  data() {
    return {
      message: "Hello",
    }
  },
  methods:{
    updateHandle(){
        this.message+=" World";
    }
  },
  watch:{
    message(newValue,oldValue){//参数分别为新值和旧值，一旦更新便会触发
        console.log(newValue,oldValue);
    }
  }
};
</script>
```

## 表单输入绑定 v-model

v-model将表单输入的数据与data中的变量绑定

```vue
<template>
    <h3>表单输入绑定</h3>
    <form>
        <input type="text" v-model="message">
        <p>{{ message }}</p>
    </form>
</template>
<script>
export default {
    data(){
        return{
            message:"",
        }
    }
}
</script>
```

复选框：

```vue
<template>
    <h3>表单输入绑定</h3>
    <form>
        <input type="text" v-model="message">
        <p>{{ message }}</p>
        <input type="checkbox" id="checkbox" v-model="checked"><span>{{ checked }}</span>
    </form>
</template>
<script>
export default {
    data(){
        return{
            message:"",
            checked:false,
        }
    }
}
</script>
```

### 修饰符

+ .lazy: 每次当失去焦点时（光标不在输入框上）才会更新

+ .trim

+ .number

### 模板引用：访问DOM元素

+ 用ref=""注册refs上的元素，用this.$refs读取

```vue
<template>
  <div ref="container">{{ content }}</div>
  <button @click="getElementHandle">获取元素</button>
</template>
<script>
export default {
  data() {
    return {
      content: "内容",
    };
  },
  methods: {
    getElementHandle() {
      console.log(this.$refs.container);
    },
  },
};
</script>
```

+ 也可以操作DOM的原生JS属性：

```vue
<template>
  <div ref="container">{{ content }}</div>
  <button @click="getElementHandle">获取元素</button>
  <input type="text" ref="textInput">
</template>
<script>
export default {
  data() {
    return {
    };
  },
  methods: {
    getElementHandle() {
      console.log(this.$refs.textInput.value);
    },
  },
};
</script>
```

上例中，点击按钮可获取input框中的内容（即用户输入的）

## 组件组成

在\<script>中引入和注册，在\<template>中引用

+ **App.vue**

```vue
<template>
  <MyComponent/>
  <my-component/><!--两种取名方式都可行-->
</template>
<script>
import MyComponent from './components/MyComponent.vue';//引入
export default{
  components:{
    MyComponent,//注册
  }
}
</script>
<style scoped>

</style>
```

+ **MyComponent.vue**

```vue
<template>
    <p>我是component</p>
</template>
<script>
export default{
    
}
</script>

```

## 组件嵌套关系

\<Root>--\<Header>

​     |-------\<Main>--\<Article>*2

​     |-------\<Aside>--\<Item>*3

 组件之间可以互相嵌套，代码略

## 组件注册方式

### 全局注册

在main.js中注册：

```
app=createApp(App)
```

和

```
app.mount('#app')
```

之间注册

```vue
import { createApp } from 'vue'
import App from './App.vue'
import MyComponent from './components/MyComponent.vue';

const app = createApp(App)

app.component("MyComponent", MyComponent)

app.mount('#app')

```

其他地方直接引用即可

+ **缺点：**

1. 没有被使用的组件无法在生产打包时自动移除
2. 大型项目中项目的依赖关系变得没那么明确。在父组件中使用子组件时，不太容易定位子组件的实现。类似使用过多全局变量，影响应用长期的可维护性

## 组件传递数据 Props

子组件接受父组件传来的数据用props

父组件在子组件处加()=""的形式传递数据

+ Parent.vue:

```vue
<template>
    <h3>Parent</h3>
    <Child title="Parent数据" demo="数据2"/><!--向子组件传递数据-->
</template>
<script>
import Child from './Child.vue'
export default {
    data(){
        return{

        }
    },
    components:{
        Child,
    }
}
</script>
```

+ Child.vue:

```vue
<template>
    <h3>Child</h3>
    <p>{{ title }}</p>
    <p>{{ demo }}</p>
</template>
<script>
export default{
    data(){
        return{

        }
    },
    props:["title","demo"]//从父组件接收数据
}
</script>
```

### 结合v-bind使用

可以传递data()中的数据，动态传递数据

+ Parent.vue:

```vue
<template>
    <h3>Parent</h3>
    <Child title="Parent数据" :demo="msg"/>
</template>
<script>
import Child from './Child.vue'
export default {
    data(){
        return{
            msg:true,
        }
    },
    components:{
        Child,
    }
}
</script>
```

### 注意事项

props传递数据，只能从父级传递到子级，不能反其道而行

### props校验

#### 设置接受类型

在子组件的接受端的props处增加数据类型，如：

```vue
props:{
    "title":String,
}
```

这样，当父组件传过来的title不为String类型时，控制台会报错

也可以接受多种数据类型：

```vue
props:{
    "title":{
        type:[Number,String,Object],
    }
}
```

#### 设置不传时的默认值

```vue
props:{
    title:{
        type:String,
    },
    age:{
        type:Number,
        default:0,
    }
}
```

当负组件不传值时，age默认为0

数字和字符串可以直接default，但是**数组和对象必须通过工厂函数获得默认值**

```vue
props:{
    title:{
        type:String,
    },
    age:{
        type:Number,
        default:0,
    },
    names:{
        type:Array,
        default(){
            return ["空"];
        }
    }
}
```

### 设置必须传

添加required:true，不传的话控制台报警

```vue
props:{
    title:{
        type:String,
    },
    age:{
        type:Number,
        required:true,
    },
    names:{
        type:Array,
        default(){
            return ["空"];
        }
    }
}
```

+ **注意事项：props是只读的**，不允许修改父元素传递的数据
+ 但可以作为参数在函数里面调用：调用方法和本地变量一样

```vue
methods:{
	handle(){
		console.log(this.names);
	}
}
```

### props通过特殊设计也可以实现子传父

+ 核心思想：父元素将某个需要参数的函数用props传给子元素（似乎是浅拷贝），然后子元素调用该函数，此时父元素处也正在调用该函数，在函数内将其赋值给父元素的某变量

+ ComponentA.vue(父组件)：

```vue
<template>
    <h3>componentA</h3>
    <ComponentB title="标题" :onEvent="datafn"/>
    <div>{{ message }}</div>
</template>
<script>
import ComponentB from './componentB.vue';
export default {
    data(){
        return{
            message:"",
        }
    },
    methods:{
        datafn(data){
            console.log(data);
            this.message=data;
        }
    },
    components:{
        ComponentB,
    }
}
</script>
```

+ ComponentB.vue(子组件)

```vue
<template>
    <h3>componentB</h3>
    <p>{{ title }}</p>
    <div>{{onEvent('传递数据')}}</div>
</template>
<script>
export default {
    data(){
        return{

        }
    },
    props:{
        title:String,
        onEvent:Function,
    }
}
</script>
```

父组件将datafn()函数传给子组件，子组件处该函数名为onEvent(但调用的其实是一个函数)，子组件调用该函数时父组件的message被赋值

## 组件事件（$emit子传父）

在子组件的某方法中设置this.$emit("")，括号内为事件名。在父组件中用@...=""来接受，...为刚刚定义的事件名，""内为触发方法（定义在methods中），当接收到信息后该方法被调用。

+ ComponentEvent.vue(父组件)：

```vue
<template>
    <h3>组件事件</h3>
    <Child @someEvent="getHandle"/>
</template>
<script>
import Child from './Child.vue';
export default {
    components:{
        Child,
    },
    methods:{
        getHandle(){
            console.log("receieved");
        }
    }
}
</script>
```

事件名称为someEvent，收到数据后触发getHandle方法

+ Child.vue(子组件)

```vue
<template>
    <h3>Child</h3>
    <button @click="clickEventHandle">传递数据</button>
</template>
<script>
export default {
    methods:{
        clickEventHandle(){
            //自定义事件
            this.$emit("someEvent");
        }
    }
}
</script>
```

点击按钮后调用clickEventHandle方法，该方法触发someEvent事件

### 在组件事件中增加参数

在emit后的括号中，第一个参数为事件名，第二个参数即为传递的内容

在接收端，事件触发调用的方法中参数即为上述传递的内容

+ Child.vue

```vue
<template>
    <h3>Child</h3>
    <button @click="clickEventHandle">传递数据</button>
</template>
<script>
export default {
    methods:{
        clickEventHandle(){
            //自定义事件
            this.$emit("someEvent","数据");
        }
    }
}
</script>
```

this.$emit中的第二个值"数据"即为传递给接受端的参数

+ ComponentEvent.vue

```vue
<template>
    <h3>组件事件</h3>
    <Child @someEvent="getHandle"/>
</template>
<script>
import Child from './Child.vue';
export default {
    components:{
        Child,
    },
    methods:{
        getHandle(data){
            console.log("receieved",data);
        }
    }
}
</script>
```

getHandle中的参数data即为接收到的数据

### 组件事件+v-model

+ 通过watch监听检测输入框中的内容变化
+ 监听到事件后触发子传父$emit



**SearchComponent.vue:**

```vue
<template>
    搜索：<input type="text" v-model="search">
</template>
<script>
export default {
    data(){
        return{
            search:"",
        }
    },
    watch:{
        search(newValue,oldValue){
            this.$emit("searchEvent",newValue);
        }
    },
}
</script>
```

**Main.vue:**

```vue
<template>
    <h3>Main</h3>
    <SearchComponent @searchEvent="getSearch"/>
    <p>{{ message }}</p>
</template>
<script>
import SearchComponent from './SearchComponent.vue';
export default {
    components:{
        SearchComponent,
    },
    data(){
        return{
            message:""
        }
    },
    methods:{
        getSearch(search){
            this.message=search;
        }
    }

}
</script>
```

## 透传Attributes

传递给一个组件，却没有被该组件声明为props或emits的attribute或者v-on事件监听器。

当一个组件以单个元素为根作渲染时，透传的attribute会自动被添加到根元素上

+ **父组件：**

```vue
<template>
    <h3>AttributeParent</h3>
    <AttrComponent class="attr"/><!--透传attribute-->
</template>
<script>
import AttrComponent from './AttrComponent.vue';
export default {
    components:{
        AttrComponent,
    }
}
</script>
```

+ 子组件：

```vue
<template>
    <h3>AttributeComponent</h3>
</template>
<script>
export default {
    
}
</script>
<style>
.attr{
    color:red;
}
</style>
```

此时class="attr"从父组件传到子组件

<mark>最重要：子组件必须有唯一根元素！</mark>如果上述例子再加一行

```html
<h4>我是新加的元素</h4>
```

则两个标签内容均不会变红

### 禁用透传Attribute

增加

```javascript
inheritAttrs:false
```

+ 子组件：

```vue
<template>
    <h3>AttributeComponent</h3>
</template>
<script>
export default {
    inheritAttrs:false,
}
</script>
<style>
.attr{
    color:red;
}
</style>
```

## 插槽slots

传递一个HTML结构

+ 父组件：

```vue
<template>
  <SlotsBase>
    <h3>插槽标题</h3>
    <p>插槽内容</p>
  </SlotsBase>
</template>

<script>
import SlotsBase from './components/SlotsBase.vue';
export default {
  components:{
    SlotsBase,
  }
}
</script>

<style>
</style>
```

+ 子组件：

```vue
<template>
    <slot></slot>
    <h3>插槽基础</h3>
    
</template>
<script>
export default {
    
}
</script>
```

\<slot>\</slot>标签用于放父元素传过来的部分

### 渲染区域

插槽内容可以访问到父组件的数据作用域，因为插槽内容本身是在父组件模板中定义的

+ 父组件：

```vue
<template>
  <SlotsTwo>
    <h3>{{ message }}</h3>
  </SlotsTwo>
</template>

<script>
import SlotsTwo from './components/SlotsTwo.vue';
export default {
  data(){
    return{
      message:"插槽内容续集",
    }
  },
  components:{
    SlotsTwo,
  }
}
</script>

<style>
</style>
```

+ 子组件：

```vue
<template>
    <h3>Slots续集</h3>
    <slot></slot>
</template>
<script>
export default {
    
}
</script>
<style>

</style>
```

可以显示message的值"插槽内容续集"

### 设置插槽默认值

在两个slot标签中间放的内容是插槽的默认值，当父元素没有传递插槽元素时，会显示默认值

如：

+ 父组件：

```vue
<template>
  <SlotsTwo>
  </SlotsTwo>
</template>

<script>
import SlotsTwo from './components/SlotsTwo.vue';
export default {
  data(){
    return{
      message:"插槽内容续集",
    }
  },
  components:{
    SlotsTwo,
  }
}
</script>

<style>
</style>
```

+ 子组件：

```vue
<template>
    <h3>Slots续集</h3>
    <slot>插槽默认值</slot>
</template>
<script>
export default {
    
}
</script>
<style>

</style>
```

此时，父组件没有传递任何结构，子组件插槽处显示"插槽默认值"

### 具名插槽

父元素处通过\<template v-slot:>或\<template #>来实现插槽的某一部分的命名，子元素通过\<slot name="">来接受对应的内容

+ 父组件：

```vue
<template>
  <SlotsTwo>
    <template v-slot:header>
      <h3>标题</h3>
    </template>
    <template #main>
      <p>内容</p>
    </template>
  </SlotsTwo>
</template>

<script>
import SlotsTwo from './components/SlotsTwo.vue';
export default {
  data(){
    return{
      message:"插槽内容续集",
    }
  },
  components:{
    SlotsTwo,
  }
}
</script>

<style>
</style>

```

+ 子组件：

```vue
<template>
    <h3>Slots续集</h3>
    <slot name="header">插槽默认值</slot>
    <hr>
    <slot name="main">插槽默认值</slot>
</template>
<script>
export default {
    
}
</script>
<style>

</style>
```

这样对同一个子组件的插槽内容就可以分割成多份，同时子组件也可以按需求随意的拜访这些部分。

### 插槽中的数据传递

有时需要插槽元素之间同时显示父组件给的内容和子组件自己的内容，这就需要子组件中内容先传递给父组件，父组件再通过插槽传递给子组件

在\<slot>标签上将子组件数据通过...="..."的形式传递给父组件，父组件用v-slot=""来接受

+ 父组件：

```vue
<template>
  <SlotsAttr v-slot="slotProps">
    <h3>{{ currentTEST }}</h3>
    <p>{{ slotProps.msg }}</p>
  </SlotsAttr>
</template>

<script>
import SlotsAttr from './components/SlotsAttr.vue';
export default {
  data(){
    return{
      currentTEST:"测试内容",
    }
  },
  components:{
    SlotsAttr,
  }
}
</script>

<style>
</style>
```

slotProps可改为其它名字，此变量为接收子元素所有传过来数据的集合对象，用点来访问子组件传过来的数据

+ 子组件：

```vue
<template>
    <h3>slots再续集</h3>
    <slot :msg="childMessage"></slot>
</template>
<script>
export default {
    data(){
        return{
            childMessage:"子组件数据",
        }
    }
}
</script>
```

首先，childMessage发送到slot上，然后父组件通过slotProps访问msg得到子组件传过去的元素

<mark>注意，这里的插槽数据传递的v-slot=""和上面的起名v-slot:不一样</mark>

### 具名插槽数据传递

核心语法：(即v-slot和=之间多加了一个:name, name是插槽一部分的名字)

```vue
v-slot:name=""
```

+ 子组件：

```vue
<template>
    <h3>slots再续集</h3>
    <slot name="name1" :msg="childMessage"></slot>
    <slot name="name2" :demo="demoMessage"></slot>
</template>
<script>
export default {
    data(){
        return{
            childMessage:"子组件数据",
            demoMessage:"子组件数据2",
        }
    }
}
</script>
```

+ 父组件：

```vue
<template>
  <SlotsAttr>
    <template #name1="slotProps">
      <h3>{{ currentTEST }}</h3>
      <p>{{ slotProps.msg }}</p>
    </template>
    <template #name2="slotProps">
      <p>{{slotProps.demo}}</p>
    </template>
  </SlotsAttr>

</template>

<script>
import SlotsAttr from './components/SlotsAttr.vue';
export default {
  data(){
    return{
      currentTEST:"测试内容",
    }
  },
  components:{
    SlotsAttr,
  }
}
</script>

<style>
</style>
```

## 组件的生命周期

每个Vue组件实例在创建时要经历异世界初始化步骤，如设置好数据帧听、编译模板、挂载实例到DOM，以及在数据改变时更新DOM。在此过程中，它也会运行名为生命周期钩子的函数，让开发者有机会在特定阶段运行自己的代码

+ 生命周期示意图：

![](.\assets\vue1.png)

```
生命周期函数：
创建期：beforeCreate  created(组件创建完成)
挂载期：beforeMounte mounted(完成渲染)
更新期：beforeUpdate updated(数据更新，再次渲染 的 循环)
销毁期：beforeUnmounte  unmounted(组件销毁)
```

页面刚开始加载完后`beforeCreate`, `created`, `beforeMounte`, `mounted`函数调用

一旦页面上数据更新，`beforeUpdate`和`updated`调用一次

+ **用到的`Vue`文件：**

```vue
<template>
  <h3>组件生命周期</h3>
  <p>{{ message }}</p>
  <button @click="updateHandle">更新数据</button>
</template>
<script>
/*
生命周期函数：
创建期：beforeCreate  created(组件创建完成)
挂载期：beforeMounte Mounted(完成渲染)
更新期：beforeUpdate updated(数据更新，再次渲染 的 循环)
销毁期：beforeUnmounte  unmounted(组件销毁)
*/

export default {

  data(){
    return{
      message:"更新之前",
    }
  },
  methods:{
    updateHandle(){
      this.message="更新之后";
    }
  },
  beforeCreate(){
    console.log("组件创建之前");
  },
  created(){
    console.log("组件创建之后");
  },
  beforeMount(){
    console.log("组件渲染之前");
  },
  mounted(){
    console.log("组件渲染之后");
  },
  beforeUpdate(){
    console.log("组件更新之前");
  },
  updated(){
    console.log("组件更新之后");
  },
  beforeUnmount(){
    console.log("组件销毁之前");
  },
  unmounted(){
    console.log("组件销毁之后");
  },
}
</script>
```



### 生命周期应用

#### 通过ref获取元素DOM结构

在生命周期函数中可以根据ref名字打印相关元素

```vue
<template>
  <h3>组件生命周期应用</h3>
  <p ref="name">百战程序员</p>
</template>
<script>
/*
生命周期函数：
创建期：beforeCreate  created(组件创建完成)
挂载期：beforeMounte Mounted(完成渲染)
更新期：beforeUpdate updated(数据更新，再次渲染 的 循环)
销毁期：beforeUnmounte  unmounted(组件销毁)
*/
import UserComponent from './components/UserComponent.vue';
import MyComponent from './components/MyComponent.vue';
export default {
  components:{
    UserComponent,
    MyComponent,
  },
  data(){
    return{
      message:"更新之前",
    }
  },
  methods:{
    updateHandle(){
      this.message="更新之后";
    }
  },
  beforeMount(){
    console.log(this.$refs.name);
  },
  mounted(){
    console.log(this.$refs.name);
  },
}
</script>
```

beforeMount()里面打印的无效，因为组件还没渲染完

mounted()里面打印的有效

#### 模拟网络请求渲染数据

在mounted()函数里将网络传过来的数据赋值给data()中的相应变量

这样就可以在渲染之后传递变量

案例略

## 动态组件

通过\<component>标签+is属性可以动态设定这个位置放的组件

```vue
<template>
    <component :is="tabComponent"></component>
    <button @click="changeHandle">切换组件</button>
</template>
<script>
import ComponentA from './ComponentA.vue';
import ComponentB from './ComponentB.vue';
export default {
    data(){
        return{
            tabComponent:"ComponentA",
        }
    },
    methods:{
        changeHandle(){
            this.tabComponent=this.tabComponent =='ComponentA'?'ComponentB':'ComponentA';
        }
    },
    components:{
        ComponentA,
        ComponentB,
    }
}
</script>
```

### 2023.11.03延申：component里的is属性究竟能放什么

+ 如上面演示的自己引入的组件

+ html元素

```vue
<component is="div">这是div</component>
<component is="input">这是input</component>
```

+ 全局注册组件（如组件库里的）

```vue
<component is="el-input">elementUI的组件</component>
```



### 组件保持存活

当使用\<component :is="">来切换组件时，被切换掉的组件会被卸载（进入卸载期），在外面包裹一个\<keep-alive>\</keep-alive>保持存活

+ 测试卸载：（父组件即为上面的代码）

```vue
<template>
    <h3>ComponentA</h3>
</template>
<script>
export default {
    beforeMounte(){
        console.log("组件卸载之前");
    },
    mounted(){
        console.log("组件卸载之后");
    },
}
</script>
```

每两次切换会打印一次组件卸载

+ 避免卸载：用\<keep-alive>\</keep-alive>标签保护

```vue
<template>
    <keep-alive>
        <component :is="tabComponent"></component>
    </keep-alive>
    <button @click="changeHandle">切换组件</button>
</template>
<script>
import ComponentA from './ComponentA.vue';
import ComponentB from './ComponentB.vue';
export default {
    data(){
        return{
            tabComponent:"ComponentA",
        }
    },
    methods:{
        changeHandle(){
            this.tabComponent=this.tabComponent =='ComponentA'?'ComponentB':'ComponentA';
        }
    },
    components:{
        ComponentA,
        ComponentB,
    }
}
</script>
```

子组件：

```vue
<template>
    <h3>ComponentA</h3>
    <p>{{ message }}</p>
    <button @click="changeData">更换数据</button>
</template>
<script>
export default {
    beforeMounte(){
        console.log("组件卸载之前");
    },
    mounted(){
        console.log("组件卸载之后");
    },
    data(){
        return{
            message:"老数据",
        }
    },
    methods:{
        changeData(){
            this.message="新数据";
        }
    }
}
</script>
```

## 异步组件

在大型项目中，我们有时可能拆分应用为更小的快，并仅在需要时再从服务器加载相关组件。Vue提供了DefineAsyncComponent来实现此功能

在父组件导入子组件的时候用如下语法：

```vue
const ComponentB=defineAsyncComponent(()=>{
    import('./ComponentB.vue');
})
```

那么只有ComponentB要用的时候才会引入

这样加快初始时导入速度

## 依赖注入

父组件向子组件传递数据需要props，但当组件有很多层，需要向多代后辈传递数据时，会很麻烦

使用provide()和inject进行跨辈传输：

+ 祖先组件：

```vue
<template>
  <h3>祖宗</h3>
</template>
<script>
import Parent from './components/Parent.vue'
export default {
  provide:{
    message:"爷爷的财产",
  },
  components:{
    Parent,
  }
}
</script>
```

+ 子组件：

```vue
<template>
    <h3>Child</h3>
    <p>{{ message }}</p>
</template>
<script>
export default {
    inject:["message"],
    props:{
        title:{
            type:String,
        },
    },
}
</script>
```

同时，依赖注入也可以传递祖先组件的动态数据

注意这里必须用provide(){return{}}的格式，不能用provide:{}的格式了

+ 祖先组件：

```vue
<template>
  <h3>祖宗</h3>
  <Parent title="祖宗的财产" />
</template>
<script>
import Parent from "./components/Parent.vue";
export default {
  data() {
    return {
      msg: "爷爷的财产!!!",
    };
  },
  provide(){
    return{
      message: this.msg,
    }
  },
  components: {
    Parent,
  },
};
</script>
```

也可以在子组件处将收到的值赋值给子组件的data：

+ 子组件：

```vue
<template>
  <h3>Child</h3>
  <p>{{ title }}</p>
  <p>{{ fullMessage }}</p>
</template>
<script>
export default {
  inject: ["message"],
  data() {
    return {
      fullMessage: this.message,
    };
  },
  props: {
    title: {
      type: String,
    },
  },
};
</script>
```

+ **温馨提示：**

Provide和inject只能由上到下的传递，不能从下到上

可以在整个应用层面提供依赖，修改main.js中的内容：

```javascript
const app=createApp(App)

app.provide("globalData","我是全局数据")

app.mount('#app')
```

这样就可以在任意地方inject

## Vue应用

### 应用实例

每个Vue应用都是通过createApp函数创建一个新的应用实例

```javascript
import { createApp } from 'vue'

const app=createApp({
  /*根组件选项*/
})

```

### 根组件

传入createApp的对象实际上是一个组件，每个应用都需要一个“根组件”，其他组件将作为其子组件

```javascript
import { createApp } from 'vue'
//导入根组件：
import App from './App.vue'
const app=createApp(App)//创建vue实例对象
```

app: Vue的实例对象

### 挂载应用

```javascript
app.mount('#app')
```

挂载在`index.html`的`id="app"`的div上



## Vue3 组合式 API

### 组合式API入口setup

```vue
<script>
export default{
  setup(){
    
  },
  beforeCreate(){

  }
}
</script>
```

`setup()`会在`beforeCreate()`钩子之前执行

#### 声明数据和函数

```vue
<template>
  App
  {{ message }}
  <button @click="logMessage">log</button>
</template>
<script>
export default{
  setup(){
    //数据
    const message='this is message';
    //函数
    const logMessage=function(){
      console.log(message);
      return message;
    }
    return{
      message,
      logMessage,
    }
  },
  beforeCreate(){

  }
}
</script>
```

记得一定要把声明的变量return出去

<mark>注意这里没有用this，Vue3不使用this，this并不指向组件实例，而是undefined</mark>

#### \<script setup>语法糖

```vue
<template>
  App
  {{ message }}
  <button @click="logMessage">log</button>
</template>
<script setup>
//数据
const message = "this is message";
//函数
const logMessage = function () {
  console.log(message);
  return message;
};
</script>
```

在\<script setup>语法糖中，组件只需要引入即可顺便注册，不需要单独写注册

### reactive()



