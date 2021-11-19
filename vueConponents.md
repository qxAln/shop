# day1

## 指令

#### 内容渲染指令

   1. ==v-text== 缺点：覆盖元素中的默认内容
   2. =={{}}== 插值表达式只能用在原数的内容节点，无法用在属性节点
   3. ==v-html== 把带有HTML标签的字符串，渲染为真正的DOM元素

#### 属性绑定指令

   1. ==v-bind ：==       
   2. 简写为==：==

#### 事件绑定指令

1. ==v-on：==简写为==@==

2. ```
   @click="show"和@click="show(传参)"
   内置变量 $event  例：@click="show(2，$event )
   ```

##### 1.事件修饰符

1. ==.prevent==    阻止默认行为（例如：阻止a链接的跳转，阻止表单的提交等）
2. ==.stop==          阻止事件冒泡
3. ==.capture==    以捕获模式触发当前的事件处理函数
4. ==.once==         绑定的事件只触发一次
5. ==.self==            只有在evet.target是当前元素自身时触发事件处理函数

##### 2.按键修饰符

1. ==.esc==
2. ==.enter==

#### 双向数据绑定指令

==v-model== 只能用在表单元素中

##### v-model指令的修饰符

1. ==.number==  自动将用户的输入值转为数值类型  

   ```
   <input v-model.number="age"/>
   ```

2. ==.trim==         自动过滤用户输入的首尾空字符串

   ```
   <input v-model.trim="msg"/>
   ```

3. ==.lazy==          在“change"时而非”input“时更新

   ```
   <input v-model.lazy="msg"/>
   ```



#### 循环渲染指令

```
v-for="(item,index) in 数组", :key="item.id"
注:拿索引当key没有意义
```



#### 条件渲染指令

v-if  动态创建和移除元素

+ v-else-if
+ v-else

v-show 动态创建和移除display：none样式

#### 过滤器

注意点：

1. 要定义到filters函数节点下，本质是一个函数
2. 在过滤器函数中，一定要有return值
3. 在过滤器的形参中，就可以获取到“管道符”前面待处理的值（管道符：==|==）

##### 1.全局过滤器

```
Vue.filter('名字',function(过滤器前面的值){ return 结果})
例如：
  Vue.filter('capi', function(str) {
            const first = str.charAt(0).toUpperCase()
            const other = str.slice(1)
            return first + other
        })
```

##### 2.私有过滤器

定义到组件的filters节点下

```
  filters: {           
                    val = val.toString()
                    return val.charAt(0).toUpperCase() + val.slice(1)
                }
            }
```

##### 3.调用

{{message | dataFormate}}

##### 4.代码示例

```
<body>
    <div id="app">
        <p>{{message | capi}}</p>
    </div>
    <div id="app2">
        <p>{{message | capi}}</p>
    </div>
    <script src="./js/vue.js"></script>
    <script>
        // 使用Vue.filter()定义全局过滤器
        Vue.filter('capi', function(str) {
            const first = str.charAt(0).toUpperCase()
            const other = str.slice(1)
            return first + other
        })
        const vm = new Vue({
            el: '#app',
            data: {
                message: 'hello vue',
            },
            //私有过滤器
            filters: {
                capi(val) {
                    const first = val.charAt(0).toUpperCase()
                    const other = val.slice(1)
                    return first + other
                }
            }
        })
        const vm2 = new Vue({
            el: '#app2',
            data: {
                message: 'hello',
            }

        })
    </script>
</body>
```

# day2

#### watch侦听器

 1.方法格式的侦听器

```
username(newVal,oldVal){}
```

```
  const vm = new Vue({
            el: "#app",
            data: {
                username: ''
            },
            watch: {
                // 所有的侦听器，都应该被定义到watch节点下
                // 侦听器本身就是一个函数，要监视哪个数据的变化，就报数据名最为方法即可
                // 新值在前旧值在后
                // 方法格式的侦听器
                // newVal 
                username(newVal) {
                   if (newVal === '') return
                        // 1.调用jQuery中的ajax发起请求，判断newVal是否被占用
                   $.get('https://www.escook.cn/api/finduser/' + newVal,function(result){
                        console.log(result);
                    })
                }
            }
```

2.对象格式的侦听器

```
username:{
   handle(newVal,oldVal){},
   deep:true,
   immediate:true
}
```

```
const vm = new Vue({
            el: "#app",
            data: {
                username: 'admin'
            },
            watch: {
                // 所有的侦听器，都应该被定义到watch节点下
                // 侦听器本身就是一个函数，要监视哪个数据的变化，就报数据名最为方法即可
                // 新值在前旧值在后
                //   定义对象格式的侦听器
                username: {
                    // 侦听器的处理函数
                    handler: function(newVal, oldVal) {
                        console.log(newVal, oldVal);
                    },
                    // 选项的默认值是false，它的作用是：控制侦听器是否自动触发一次
                    immediate: true
                }
            }
        })
```

#### 计算属性computed

1. 在定义的时候，要定义为function
2. 在使用的时候，当做普通属性使用即可
   + 在template模板结构中也可以使用
   + 在methods方法中，也可以使用（this.计算属性的名字）
3. 要return一个计算的结构
4. 只要任何一个依赖的数据项发生了变化，计算属性就会重新复制

```
  const vm = new Vue({
            el: '#app',
            data: {
                r: 0,
                g: 0,
                b: 0
            },
            // 所有的计算属性都要写在computed节点之下
            // 计算属性在定义的时候，要定义成“方法格式”
            computed: {
                // rgb作为一个计算属性，被定义了方法格式
                // 最终，在这个方法中，要返回一个生成好的rgb（x，x，x）的字符串
                rgb() {
                    return `rgb(${this.r},${this.g},${this.b})`
                }
            },
            methods: {
                show() {
                    console.log(this.rgb)
                }
            }
```

#### vue-cil

##### 1.全局安装  npm install @vue/cil -g

##### 2.创建项目  vue create 项目名称

##### 3.组件的构成

+ template 只能有唯一的根节点
+ script   
  + xport default{} 
  +   .vue 中的data必须是方法
+ style   
  + 用less语法-----lang=“less”
  + 防止组件之间样式冲突 
    + scoped
    + /deep/ 深度选择器-------在需要覆盖第三方组件样式的时候会用到

```
1.template 组件的模板结构
2.script 组件的JavaScript行为
3.style 组件的样式


<template></template>
<script></script>
<style></style>
```

##### 4.组件使用步骤

```
1.使用import语法导入需要的组件
    import xxx from '@/components/xxx.vue'
2.使用components节点注册组件
    export default{
       components:{
          xxx
        }
    }
3.以标签形式使用刚才注册的组件
   <div>
      <xxx></xxx>
    </div>
4.代码：
<template>
  <div id="app">
    <h1>App根组件</h1>
    <!-- 3.以标签形式使用注册的组件 -->
    <left></left>
    <right></right>  
  </div>
</template>

<script>
// 1.导入需要使用的.vue 组件
import left from '@/components/left.vue'
import right from '@/components/right.vue'


export default {
  // 2.注册组件
    components:{
    left,right
  } 
}
</script>
```



#### 注册全局组件

   ==注==：在vue项目的main.js入口文件中，通过vue.component()方法，可以注册全局组件

1.导入需要全局注册的组件

 ```
import xxx from '@/component/xxx.vue'
 ```

2.通过vue.component()方法，可以注册全局组件,在main.js 注册组件

```
vue.component('xxx',xxx)
参数1:字符串格式，表示组件的“注册名称”
参数2：需要被全局注册的那个组件
```



# day3

#### props组件

特点：极大的提高组件的复用性



+  props是自定义属性，运行使用者通过自定义属性，为当前组件指定初始值
+   自定义属性的名字，是封装者自己定义的（只要合法就行）
+ props中的数据，可以直接在模板结构中被使用
+ vue规定：组件中封装的自定义属性是只读的，程序员不能直接修改props的值，否则会报错
+ 要想修改props的值，可以把props的值转缓存到data当中，因为data的数据都是可读可写的

定义props有两种格式

1. 数组格式

   ```
   props:['init']
   ```

   

2. 对象格式

   ```
      props:{
           // 自定义属性A：{/* 配置选项*/}
           // 自定义属性B：{/* 配置选项*/}
           // 自定义属性C：{/* 配置选项*/}
           init:{
               // 如果外界使用count组件的时候，没有传递init属性，则默认值生效
               default:0,
               // init的值类型必须为Number数字
               type:Number,
               // 必填项校对
               required:true
           }
   
       }
   ```

   

#### 生命周期

概念：每个组件从创建-->运行-->销毁的一个过程，强调的是一个时间段

#### 生命周期函数

概念：在生命周期的不同阶段，会按次序，自动依次执行的函数，叫做生命周期函数。强调的是时间点

##### 三个阶段

###### 1.创建阶段

```
beforeCreate
*** created:发起Ajax最早的时机，请求数据
beforeMount
*** mounted：组件第一次被渲染到浏览器，操作DOM的最早的时机
```



###### 2.运行阶段

```
beforeUpdate
*** updated:能够操作到最新的DOM 元素
```



###### 3.销毁阶段

```
beforeDestroy
destoryed
```



#### 组件之间的数据共享

##### 1.父--->子 

​     父组件向子组件共享数据使用自定义属性

          + 子组件中，通过props来自定义属性
          + 父组件中，负责把数据通过v-bind：绑定给子组件

##### 2.子--->父

​     子组件向父组件共享数据使用自定义事件

   + 子组件中，调用this.$emit()来触发自定义事件
     + 参数1：字符串，表示自定义事件的名称
     + 参数2：值，要发送给父组件的数据
   + 父组件中，通过v-on：来绑定自定义事件，并提供一个数据处理函数。通过事件处理函数的形参，接收到子组件传递过来的数据

##### 3.兄弟组件共享数据

​     兄弟组件共享数据使用EventBus

   + 数据发送方

     + 导入eventBus.js

     ```
     import bus from './eventBus.js'
     ```

     + bus.$emit('要触发事件的名字',要发送的数据)

       ```
       bus.$emit('share',this.str)
       ```

   + EventBus模块

     + 创建vue的实例对象 new Vue（）
     + 向外导出Vue的实例对象  

     ```
     export default new Vue()
     ```

+ 数据接收方

  + 导出eventBus.js

    ```
    import bus from './eventBus.js'
    ```

  + bus.$on('要声明的自定义事件的名字'，事件的处理函数)

    ```
     bus.$on('share',()=>{})
    ```

    

  + 通过数件处理函数的形参，可以接收到发送过来的数据

  + 在数据接收方的created生命周期函数中，调用bus.$on()方法

    ```
     created(){
            // 2.为bus绑定自定义事件
                bus.$on('share',(val)=>{
                        this.msgFromLeft=val
                })
            
        }
    ```

    



#### 基于axios请求列表数据

1.安装并在App中导入axios

2.在methods方法中，定义initCartList函数请求列表数据

3.在created生命周期函数中，调用步骤2封装的initCartList函数

#### ref引用

ref用来辅助开发者在不依赖与jQuery的情况下，获取DOM元素或者组件的引用

每个vue的组件实例，都包含一个$refs对象，里面存储着对应的DOM元素或者组件的引用。

默认情况下，组件的$refs指向一个空对象

#### this.$nextTick(cb)

组件的$nextTick(cb)方法，会把cb回调推迟到下一个DOM更新周期之后执行

```
this.$nextTick(()=>{})
```



# day4

#### 动态组件

动态组件指的是动态切换组件的显示与隐藏

#### component组件

```
<component></component>
```

专门用来实现动态组件的渲染，组件的占位符

is属性的值，表示要渲染组件的名字

is属性的值，应该是组件在components节点下的注册名称

```
<component :is="left"></component>
```

#### keep-alive组件

把内部的组件进行缓存，而不是销毁组件

```
 <keep-alive >
        <component :is="left"></component>
 </keep-alive>
```



#### keep-alive对应的生命周期函数

当组件被缓存时，会自动触发组件的==deactivated==生命周期函数

当组件被激活时，会自动触发组件的==activated==生命周期函数

当组件第一次被创建的时候，既会执行==created==生命周期，也会执行==activated==生命周期

#### keep-alive组件

##### include属性

include属性用来指定：只有名称匹配的组件会被缓存，多个组件名之间使用逗号分隔

```
 <keep-alive include="Left">
        <component :is="comName"></component>
 </keep-alive>
```

##### exclude属性

exclude属性用来指定：只有名称匹配的组件不会被缓存，多个组件名之间使用逗号分隔

```
 <keep-alive exclude="Left">
        <component :is="comName"></component>
 </keep-alive>
```

include属性和exclude属性不能被同时使用

#### 插槽slot

插槽是vue为组件的封装者提供的能力，运行开发者在封装组件时，把不确定的希望由用户指定的部分定义为插槽

  默认情况下，在使用组件的时候，提供的内容都会被填充到名字为default的插槽之中
       1.如果要把内容填充到指定名称的插槽中，需要使用v-slot：这个指令
        2.v-slot：后面跟上插槽的名字 
        3.v-slot：指令不能直接用在元素身上，必须用在template标签上 
        4.template这个标签，她是一个虚拟的标签，只起到包裹性质的作用，但是，不会被渲染为任何实质性的HTML元素 
       5.v-slot：指令的简写形式是# 

```
  App组件
  <Left>
      
        <template #default>
          <p>这是在Left组件中的内容区域，声明党的p标签</p>
        </template>
      </Left>
      //另一个需要放卡槽的地方
   <slot name="author"></slot>
```



#### 私有定义指令

在每个vue组件中，可以在directives节点下声明私有自定义指令

```
 // 私有自定义指令的节点
  directives:{
    // 定义名为color的指令，指向一个配置对象
    color:{
      // 当指令第一次被绑定到元素上的时候，会立即触发bind函数
      // 形参中的el表示当前指令所绑定到的那个DOM对象
      bind(el,binding){
        el.style.color=binding.value
        console.log(binding);
      },
      // 在DOM 更新的时候，会触发update函数
      update(el,binding){
        el.style.color=binding.value
      }
    }
```

如果bind和update函数中的逻辑完全相同，则对象格式的自定义指令可以简写成函数格式

```
  directives:{
    color(el,binding){
        el.style.color=binding.value
      }
    }
```

#### 全局自定义指令

全局共享的自定义指令需要通过“Vue.directive()”进行声明，在main.js里面声明

```
Vue.directive('color', function(el, binding) {
    el.style.color = binding.value
})
```



# day5

#### 1.什么是路由

路由就是对应关系

#### 2.SPA与前端路由

SPA指的是一个web网站只有唯一的一个人HTML页面，所有组件的展示与切换都在这唯一的一个页面内完成，此时，不同组件之间的切换需要通过前端路由来实现

结论：在SPA项目中，不同功能之间的切换，要依赖于前端路由来完成

#### 3.前端路由

hash地址与组件之间的对应关系

#### 4.前端路由的工作方式

①.用户点击了页面上的路由链接

②.导致了URL地址栏中的Hash值发生了变化

③.前端路由监听到了Hash地址的变化

④.前端路由把当前Hash地址对应的组件渲染到浏览器中

#### 5.vue-router安装和配置

①.安装vue-router包

```
npm i vue-router
```

②.创建路由模块

```
在src源代码目录下，新建router/index.js路由模块
// src/router/index.js 就是当前项目的路由模块
//1.导入Vue和VueRouter的包
import Vue from 'vue'
import VueRouter from 'vue-router'
import Hoem from '@/components/Home.vue'
//2.调用Vue.use()函数，把VueRouter安装Vue的插件
// Vue.use()函数的作用就是用来安装插件的
Vue.use(VueRouter)
// 3.创建路由的实例对象
const router=new VueRouter({
 routes:[
 {path:'/home',component:Home}
 ]
})
// 4.向外共享路由的实例对象
export default router
```

③.导入并挂载路由模块

```
在main.js导入并挂载
// 导入路由模块，目的：拿到路由的实例对象
// 在进行模块化导入的时候，如果给定的是文件夹，则默认导入这个文件夹下，名字叫做index.js的文件
import router from '@/router/index.js';

new Vue({
  render: h => h(App),
  // 在vue项目中，要想把路由用起来，必须把路由实例对象，通过下面的方式进行挂载
  // router:路由的实例对象
  // 由于router和路由实例对象的名称一样，所有可以省略直接简写为router
  router
}).$mount('#app')
```

④.声明路由链接和占位符

```
//占位符
<router-view></router-view>
```

当安装和配置了vue-router后，就可以使用router-link来代替普通的a链接了

```
<router-link to="/home">首页</router-link>
```

 

注意1:在hash地址中，/后面的参数项，叫做“路径参数” 

​           在路由参数对象中，需要使用this.$route.params来访问路径参数 

注意2:在hash地址中，？后面的参数项，叫做“查询参数 ”

​           在路由参数对象中，需要使用this.$route.query来访问查询参数 

注意3:在this.$route中，path只是路径部分，fullpath是完整的地址

例如：/movie/2?nama=zs&age=20是fullpath

​           /movie/2是path的值

#### 6.路由重定向

用户在访问地址A的时候，强制用户跳转到地址C，从而展示特定的组件页面，通过路由规则的redirect属性，指定一个新的路由地址，可以很方便地设置路由的重定向

```
{path:'/',redirect:'/home'}
```

#### 7.嵌套路由

通过children属性声明子路由规则

在src/router/index.js路由模块中，导入需要的组件，并使用children属性声明子路由规则

```
const router=new VueRouter({
 routes:[
 {path:'/home',component:Home},
 {path:'/movie/:mid',component:Movie},
 {
   path:'/about',
   component:About,
   redirect:'/about/tab1',
   children:[
    // 声明子路由规则
    // 默认子路由：如果children数组中，某个路由规则的path值为空字符串，则这条路由规则，叫做“默认子路由”
        {path:'tab1',component:Tab1},
        {path:'tab2',component:Tab2}
      ]
    },
 ]
})
```

#### 8.动态路由匹配

this.$route是路由的“参数对象” 

this.$router是路由的“导航对象” 

动态路由指的是：把Hash地址中可变的部分定义为参数项，从而提高路由规则的复用性

在vue-router中，使用英文的冒号(==:==)来定义路由的参数项

```
如：{path:'/movie/:id',component；movie}
```

获取id

①：用this.$route.params.id可以获取id的值

②：可以为路由规则开启props传参，从而方便的拿到动态参数的值

+  {path:'/movie/:id',component；movie,props:true}
+   props:['id']

#### 9.vue-router中的编程式导航API

##### ①this.$router.push('hash地址')

​      跳转到指定hash地址，并增加一条记录

##### ②this.$router.repalce('hash地址')

​      跳转到指定hash地址，并替换掉当前的历史记录

##### ③this.$router.go(数值n)

​     可以在浏览器历史中前进或后退

```
例如：<button @click="goBack">后退</button>
     .............
     methods:{
        goBack(){
        //后退到之前的组件页面
        //-1相当于后退一个页面
           this.$router.go(-1)
          }
     }
```

##### ④this.$router.go(数值n)的简化用法

$router.back() 在历史记录中，后退到上一个页面

$router.forward() 在历史记录中，前进到下一个页面

```
<!-- 在行内使用编程式导航跳转的时候，this必须要省略，否则会报错 -->
<button @click="$router.back() ">后退</button>
```

#### 10.导航守卫

导航守卫可以控制路由的访问权限

##### 全局前置守卫

每次发生路由的导航跳转时，都会触发全局前置守卫，因此，在全局前置守卫中，程序员可以对每个路由进行访问权限的控制

```
在index.js中声明
//调用路由实例对象的beforeEach方法，即可声明“全局设置守卫”
//每次发生路由导航跳转的时候，都会自动触发fn这个“回调函数”
router.beforeEach(fn)
```

全局前置守卫的回调函数中接收3个形参，格式为

```
router.beforeEach(function(to,from,next){})
//to是将要访问的路由的信息对象
//from是将要离开的路由的信息对象
//next是一个函数，调用next()表示放行，允许这次路由导航
```

```
例如：
// 为router实例对象，声明全局前置导航守卫
// 只要发生了路由的跳转，必然会触发beforeEach指定的function回调函数
router.beforeEach(function(to,from,next){
  // 分析
  // 1.要拿到用户将要访问的hash地址
  // 2.判断hash地址是否等于/main（如果等于/main，证明需要登录之后才能访问成功，如果不等于/main，则不需要登录，直接放行next（））
  // 3.如果访问的地址是/main，则需要读取localStorage中的token值（如果有token值，则放行。否则强制跳转到/login登录页面）
  if(to.path==='/main'){
    const token=localStorage.getItem('token')
    if (token) {
      next()
    } else {
      next('/login')
    }
  }else{
    next()
  }
})
```

