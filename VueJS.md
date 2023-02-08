# vue

    流行的前端框架
    渐进式js框架
    尤雨溪
    声明式渲染——组件系统——客户端路由——集中式状态管理(vue-x)——项目构建

     <div id="app">
        <div>{{msg}}</div>
    </div>

    <script src="js/vue.js"></script>
    <script>
       var vm = new Vue({
           el: '#app',
           data: {
                msg: 'hello Vue'
           }
       })
    </script>

    el: 元素的挂载位置
    data: 模型数据（对象）

    前端渲染：把数据填充到html标签

## 双向数据绑定

    用户修改页面内容会影响代码的数据，修改代码的数据也会影响页面的内容

## MVVM

    model——view——view-model
    视图到模型  事件监听
    模型到视图  数据绑定

## vue 指令

    本质是自定义属性  由v-开头


    数据的响应式：
        h5中响应指的是屏幕尺寸导致标签样式的变化
        在vue中是数据的改变导致页面的变化

    数据绑定：
        将数据填充到标签中


### v-clock

    在插值表达式所在的标签添加v-clock指令

     <style>
        [v-clock] {
            display: none;
        }
    </style>

    <div id="app">
        <div v-clock>{{msg}}</div>
    </div>

    <script src="js/vue.js"></script>
    <script>
       var vm = new Vue({
           el: '#app',
           data: {
                msg: 'hello Vue'
           }
       })
    </script>

### 填充数据的三个指令

    v-text      填充纯文本
    v-html      填充html片段(存在安全问题，本网站的数据可以直接使用，第三方的数据不可以用)
    v-pre       填充原始信息  跳过编译信息

#### v-text

     <div id="app">
        <div v-text="msg"></div>
    </div>

    <script src="js/vue.js"></script>
    <script>
       var vm = new Vue({
           el: '#app',
           data: {
                msg: 'hello Vue'
           }
       })
    </script>

#### v-html

    会有跨域攻击
    <div id="app">

        <div v-html="msg1"></div>
    </div>

    <script src="js/vue.js"></script>
    <script>
       var vm = new Vue({
           el: '#app',
           data: {

                msg1: '<h1>hello</h1>'
           }
       })
    </script>

#### v-pre

    <div id="app">
        <div v-text="msg"></div>
        <div v-html="msg1"></div>
        <div v-pre>{{msg}}</div>
    </div>

    <script src="js/vue.js"></script>
    <script>
       var vm = new Vue({
           el: '#app',
           data: {
                msg: 'hello Vue',
                msg1: '<h1>hello</h1>'
           }
       })
    </script>

### v-once

    解除数据绑定，当通过编译后就不再修改
    用在不用修改的信息，可以提高性能

### v-model

    数据双向绑定

#### 本质:

        通过input事件
                    <div id="app">
                        <div>{{msg}}</div>
                        <div>
                            <input type="text" :value="msg" @input="input">
                            <input type="text" :value="msg" @input="msg=$event.target.value">
                            <input type="text" v-model="msg">
                        </div>
                    </div>

                    <script src="js/vue.js"></script>
                    <script>
                        var vm = new Vue({
                            el: '#app',
                            data: {
                                msg: 'hello'
                            },
                            methods: {
                                input: function(event) {
                                    this.msg = event.target.value
                                }
                            }
                        })
                    </script>

     <div id="app">
        <div v-text="msg"></div>
        <div v-html="msg1"></div>
        <div>{{msg}}</div>
        <div>
            <input type="text" v-model="msg">
        </div>
    </div>

    <script src="js/vue.js"></script>
    <script>
       var vm = new Vue({
           el: '#app',
           data: {
                msg: 'hello Vue',
                msg1: '<h1>hello</h1>'
           }
       })
    </script>

### v-on

    简写为 @
    事件绑定

        <div id="app">
        <div>
            {{msg}}
        </div>
        <button v-on:click="msg++">加一</button>
        </div>

        <script src="js/vue.js"></script>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    msg: 0
                }
            })
        </script>

    或者使用@
        <div id="app">
            <div>
                {{msg}}
            </div>
            <button @click="msg++">加一</button>
        </div>

        <script src="js/vue.js"></script>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    msg: 0
                }
            })
        </script>

#### 函数调用

    @click="handle"
    @click="handle()"
    区别在于 第二种可以传其他参数  第一种只传递事件对象
    @click="handle(123,'aa',$event)"  $event固定写法，并且在最后一个

    <div id="app">
        <div>
            {{msg}}
        </div>
        <button @click="msg++">加一</button>
        <button @click="handle">加一</button>

    </div>

    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                msg: 0
            },
            methods: {
                handle: function () {
                    this.msg++  //this是vue的实例对象 vm
                }
            }
        })
    </script>

#### 事件修饰符

    .stop阻止冒泡
    @click.stop="handle"

    .prevent阻止默认行为
    @click.prevent="handle"


    .self
    只有点击本身的元素才会触发

##### 按键修饰符

    v-on:keyup.enter

    <form action="#">
            <div>
                用户名：<input type="text" @keyup.delete="clear" v-model="uname">
                密  码：<input type="text" @keyup.enter="submit" v-model="pwd">
            </div>
            <div>
                <input type="button" @click="submit" value="提交">
            </div>
        </form>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                uname: '张三',
                pwd: 123
            },
            methods: {
                submit: function() {
                    console.log(this.uname,this.pwd);
                },
                clear: function() {
                    this.uname = ''
                }
            }
        })
    </script>

###### 自定义一个案件修饰符

    Vue.config.keyCodes.f1 = 112(每个按键都有一个唯一标识)

### v-bind

    简写为 :
     <div id="app">
        <a v-bind:href="url">百度</a>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                url: 'http://www.baidu.com'
            }
        })
    </script>

### class 样式绑定

    对象绑定和数组绑定可以同时使用
    class绑定的值可以简化
        可以在模板中定义classes  然后再js代码中指明
    默认的class
        默认的class不会被覆盖

#### 对象绑定类

    <style>
        .active {
            border: 1px solid red;
            height: 100px;
            width: 100px;
        }

        .error {
            background-color: skyblue;
        }
    </style>
     <div id="app">
        <div :class="{active: isActive,error: isError}">test</div>
        <button @click="change">切换</button>
    </div>


    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                isActive: true,
                isError: true
            },
            methods: {
                change: function() {
                    this.isActive = !this.isActive
                }
            }
        })
    </script>

#### 数组绑定类

    <style>
        .active {
            border: 1px solid red;
            height: 100px;
            width: 100px;
        }

        .error {
            background-color: skyblue;
        }
    </style>

     <div id="app">
        <div :class="[activeClass,errorClass]">test</div>
        <button @click="change">切换</button>
    </div>


    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                activeClass: active,
                errorClass: error
            },
            methods: {
                change: function() {
                    this.activeClass = ''
                }
            }
        })
    </script>

#### 对象和数组组合使用

     <style>
        .active {
            border: 1px solid red;
            height: 100px;
            width: 100px;
        }

        .error {
            background-color: skyblue;
        }

        .test {
            color: white;
        }
    </style>

    <div id="app">
        <div :class="[activeClass,errorClass,{test: isTest}]">test</div>
        <button @click="change">切换</button>
    </div>


    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                activeClass: 'active',
                errorClass: 'error',
                isTest: true
            },
            methods: {
                change: function() {
                    this.isTest= !this.isTest
                }
            }
        })
    </script>

### style 样式绑定

    <div id="app">
        <div :style="{border: borderStyle,width: widthStyle,height: heightStyle}"></div>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                borderStyle: '1px solid red',
                widthStyle: '100px',
                heightStyle: '100px'
            }
        })
    </script>

简化
<div id="app">
<div :style="objStyles"></div>
</div>

    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                objStyles: {
                    border: 1px solid red,
                    height: 100px,
                    width: 100px
                }

            }
        })
    </script>

### 分支结构

    v-if    控制元素是否渲染，其实就是控制dom渲染过程
    v-else
    v-else-if
    v-show  控制元素的 display属性

    <div id="app">
        <div v-if="score >= 90">优秀</div>
        <div v-else-if="score<90 && score>=80">良好</div>
        <div v-else-if="score<80 && score>=70">中等</div>
        <div v-else-if="score<70 && score>=60">及格</div>
        <div v-else>不及格</div>
        <div v-show="flag"></div>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                score: 3,
                flag: false
            }
        })
    </script>

### 分支循环结构

    v-for

    key是用来帮助vue提高性能，唯一性

    <div id="app">
        <div>
            <ul>
                <li v-for="item in fruits">{{item}}</li>
                <li :key="item.index" v-for="(item,index) in fruits">{{item}}---{{index}}</li>
                <li v-for="item in myFruits">
                    <span>{{item.cname}}</span>
                    <span>{{item.ename}}</span>
                </li>
            </ul>
        </div>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                fruits: ['apple','banana','peach'],
                myFruits: [
                    {
                        ename: 'apple',
                        cname: '苹果'
                    },
                    {
                        ename: 'apple',
                        cname: '苹果'
                    },
                    {
                        ename: 'apple',
                        cname: '苹果'
                    }
                ]
            }
        })
    </script>

## vue 特性

### 表单操作

    <div id="app">
        <form action="#">
            <div>
                <span>姓名</span>
                <span>
                    <input type="text" v-model="uname">
                </span>
            </div>
            <div>
                <span>性别</span>
                <span>
                    <input type="radio" name="" id="male" value="1" v-model="gender">
                    <label for="male">男</label>
                    <input type="radio" name="" id="female" value="2" v-model="gender">
                    <label for="male">女</label>
                </span>
            </div>
            <div>
                <span>爱好</span>
                <input type="checkbox" name="" id="male" value="1" v-model="hobby">
                <label for="male">听歌</label>
                <input type="checkbox" name="" id="male" value="2" v-model="hobby">
                <label for="male">跳舞</label>
                <input type="checkbox" name="" id="male" value="3" v-model="hobby">
                <label for="male">游戏</label>
            </div>
            <div>
                <span>职业</span>
                <select v-model="occupation">
                    <option value="0">请选择职业....</option>
                    <option value="1">教师</option>
                    <option value="2">学生</option>
                    <option value="3">个人</option>
                </select>
            </div>
            <div></div>
            <div>
                <input type="submit" value="提交" @click.prevent="handle">
            </div>
        </form>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                uname: '请输入姓名',
                gender: 1,
                hobby: ['2','3'],
                occupation: 2
            },
            methods: {
                handle: function() {
                    console.log(this.occupation);
                }
            }
        })
    </script>

#### 表单域修饰符

    .number  将输入转化为数值型  （表单默认输入为字符型）
    .trim    将输入的前后空格去掉
    .lazy    将input事件转化为change事件   ，input事件是只要内容改变就会出发  change是失去焦点才会触发   （有时用在验证）

### 自定义指令

    获取元素焦点
    Vue.directive('focus',{
        inserted: function(el) {  //inserted 是钩子函数
            el.focus();  //el表示指令绑定的元素
        }
    })

### 局部指令

    在vue实例中添加属性  只在该实例范围有效
    directives: {
        focus: {
            inserted: function(el) {
                el.focus();
            }
        }
    }

### 计算属性

    就是将计算逻辑简洁化
    !!! 需要有返回值
    计算属性和方法的区别：
        方法不存在缓存，计算属性是基于他们的以来进行缓存
    computed: {
        reverseString: function() {
            return this.split('').reverse().join('')
        }
    }

### 侦听器

    ！！！ 用于异步和大批操作
    用来侦听数据（data）的变化，如果变化就通知所绑定的方法
    watch: {
        firstName（data里面的key）: function(val) {
            this.fullName = val + this.lastName;
        },
        lastName: function(val) {
            this.fullName = this.firstName + val;
        }
    }

### 过滤器

    格式化数据
    !!! 需要返回值
    <div > {{msg | 过滤器名称}} </div>  可以连续调用多个过滤器
    Vue.filter('',function(){

    })

    可以带参数，但是要从第二个开始  因为第一个默认为调用的元素
    Vue.filter('',function(value,arg1...){

    })

#### 局部过滤器

    filters: {
        名字: function() {

        }
    }

### 实例的生命周期

    挂载——更新——销毁
    挂载：初始化相关属性
        beforeCreate
        created
        beforeMount
        mounted  代表初始化完成，有了模板
    更新：元素或者组件的变更操作
        beforeUpdate
        updated
    销毁：销毁相关属性
        beforeDestroy
        destroyed

### 数组相关 api

#### 变异方法

    影响原来数据
    push()
    pop()
    shift()
    unshift()
    splice()
    sort()
    reverse()

#### 替换数组

生成新的数组

    filter()
    concat()
    slice()

#### 响应式修改数组

    Vue.set(vm.items,indexOfltem,newValue)
    vm.$set(vm.items,indexOfltem,newValue)
        vm.items    数组名字/对象名字
        indexOfltem  索引/属性名
        newValue    新的值/属性值

## 组件化开发

    标准规范、分治、重用、组合
    web components
        封装好的功能的自定义元素

## 全局组件注册

    Vue.component(组件名称,{
        data:组件数据,                      //data 必须是一个函数
        template:组件                       //模板必须只有一个根元素     模板内容可以是模板字符串  模板字符串就是``反引号包含的h5代码
        模板内容
    })
    使用方法
        <div id="app">
            <模板名称></模板名称>
        </div>

### 组件的命名方法

    驼峰式：
        HelloWorld
        MyComponent

    短线式：
        my-component



    驼峰式命名，使用时只能在模板内容中使用驼峰式的命名，在根组中必须是短线式

## 局部组件注册

    局部组件只能在注册的父组件中使用

    <script>
        var ComponentA = {
            data: function() {
                return {
                    msg: '你好'
                }
            },
            template: '<div>{{msg}}<>'
        }/div
        var ComponentB = {

        }


        var vm = new Vue({
            el: '#app',
            data: {

            },
            components: {
                'component-a': ComponentA,
                'component-b': ComponentB,

            }
        })
    </script>

## 父组件向子组件传值

    组件内部通过props接受传递来的值
    Vue.component('vue-test',{
            props: ['value1','value2'...],
            data: function() {
                return {
                    msg: 'hello'
                }
            },
            template: '<div>{{msg}}</div>'
        })


    父组件向子组件传值:
        <vue-test title="hello" :title="title"></test>

!!! 在 props 中采用驼峰式，但是在 h5 中一定要短横线
props 传递数据是单向的

## 子组件向父组件传值

    $emit

## 兄弟组件传值

    事件中心

    var eventHub = new Vue()

    eventHub.$on=('add',add() {

    })
    eventHub.$off('add')  //销毁

## 单文件组件

    在项目的src目录下创建components文件，里面创建App.vue
    里面包含三部分
        <template></template>  //组件模板区域
        <script>               //业务逻辑区域
            export module {
                data() {       //私有数据
                    return ()
                },
                methods: {

                }
            }
        </script>
        <style scoped></style>  //样式区域    scoped防止样式冲突

## promise

### restful 形式的 url

    与请求方法密切相关
    get     查询
    post    增加
    put     修改
    delete  删除

### promise 处理异步编程

    promise是一个函数对象

    var p = new Promise(function(resolve,reject) {
        //成功时resolve
        var flag = true
        if(flag) {
            resolve('你好')
        },else{
            reject('error')
        }
        //失败时reject
    })

    p.then(function(ret) {
        //从resolve得到信息
    }, function(ret) {
        //从reject得到错误信息
    })



    all方法时并发处理多个请求，所有任务都完成才能得到结果
    race方法是并发处理多个任务，只要有一个任务完成就可以得到结果，并且不再执行剩余的任务









## fetch
    升级版的ajax
    基于promise实现

    fetch(url).then(fn1)
            .then(fn2)
            .catch(fn3
                )




## axios
专门用于调用接口的js库
支持浏览器和node
支持promise
可以拦截亲请求和响应
自动转换json数据

    axios.get('url',{
                params: {
                    id: 789
                }
            })
            .then(ret => {
                consloe.log(ret.data)//data是固定的
            })


     axios.post('url',{             //post传递的是json格式的数据  在后台接口要用req.body里面的属性来调用
                name: 'jzh',
                id: 789,
                tel: 13783872126,
                pwd: 789
            })
            .then(ret => {
                consloe.log(ret.data)//data是固定的
            })



    /post的另一种参数方式   这种提交的是字符串形式的数据
    cosnt params  = new URLSearchParams();
    params.append('uname','jzh');
    axios.post('url',params)
                    .then(ret => {
                        console.log(ret.data)
                    })




     axios.delete('url',{
                params: {
                    id: 789
                }
            })
            .then(ret => {
                consloe.log(ret.data)//data是固定的
            })



### axios的响应结果
    axios将返回结果包装成一个对象
    object: {
        data:   响应得到真实数据    
        headers:    响应头信息
        status: 响应码
        statusText: 响应状态信息
    }


### axios的全局配置
    axios.defaults.timeout = 3000;  超时时间
    axios.defaults.baseURL = 'url'  默认地址
    axios.defaults.headers['mytoken'] = ''  默认请求头




### axios拦截器

#### 请求拦截器
    axios.interceptors.request.use(function(config){
        //配置内容
        return config;
    },function(err){
        consloe.log(err)
    })

#### 响应拦截器
   axios.interceptors.response.use(function(res){
        //配置内容
        return res;
    },function(err){
        console.log(err)
    })



### async和await函数
    es7引入的新语法
    async关键字用于函数上，返回值是promise的实例对象
    await关键字用于async函数当中，得到异步处理的结果
    async function queryData(id) {
        const ret = await axios.get(url);
        return ret;
    }
    queryData.then( ret => {
        console.log(ret)
    })



### async处理多个异步请求



## 路由

    后端路由
        根据请求的url对应相应的资源
    SPA 单页面应用程序
    前端路由
        根据不同的用户事件显示不同的内容
        用户事件和事件处理函数的关系
        负责事件监听，触发事件后根据处理函数渲染不同的内容
        基于url中的hash值的变化控制组件的切换

    
### vue-router
    router-link是vue提供的标签  默认渲染为a标签
    to属性渲染为href属性
    to属性的值会被默认渲染为#开头的hash地址


    <router-link to="/user">User</router-link>
    <router-link to="/register">Register</router-link>

    
路由占位符
    将来匹配到的渲染到这里
    <router-view></router-view>



路由规则
    const router = new VueRouter({
            routes: [
                {path:'/user',component: user},   //path这里要和to对应
                {path:'/register',component: register}
            ]
        })


### 路由重定向
    在用户访问地址a时，强制访问另一个地址，路由规则的redirect
     const router = new VueRouter({
            routes: [
                {path:'/',redirect: '/user'},
                {path:'/user',component: user},
                {path:'/register',component: register}
            ]
        })


### 嵌套路由

    在父路由的组件模板中添加新的router-link
     const router = new VueRouter({
            routes: [
                {path:'/',redirect: '/user'},
                {path:'/user',component: user},
                {path:'/register',component: register,children: [
                    {path:'/register/tab1',component: tab1},
                    {path:'/register/tab2',component: tab2}
                ]}
            ]
        })


### 动态路由匹配

    <router-link to="/user/1">User</router-link>
    <router-link to="/user/2">User</router-link>
    <router-link to="/user/3">User</router-link>
    {path:'/user/:id',component: user}

    在绑定的组件中可以调用这个参数
        $router.params.你起的名字来访问



### 路由组件传递参数
    {path:'/user/:id',component: user,props: true}

     const user = {
            props: ['id'],
            template: '<h1>{{id}}</h1>'
        }



        props也可以传递对象

        {path:'/user/:id',component: user,props: {uname: 'lisi',age: 17}}

        const user = {
                props: ['uname','age'],
                template: '<h1>{{name ---  age}}</h1>'
            }



         props也可以传递函数

        {path:'/user/:id',component: user,props: router => {uname: 'lisi',age: 17,id: router.params.id}}

        const user = {
                props: ['id','uname','age'],
                template: '<h1>{{id --- name ---  age}}</h1>'
            }



### 命名路由

    给路由规则起别名

    <router-link :to="{name: 'user',params: {id}}">User</router-link>
    {path:'/user/:id',component: user,name: 'user'}


### 编程式导航


     {path:'/user/:id',component: user,props: router => {uname: 'lisi',age: 17,id: router.params.id}}

        const user = {
                props: ['id','uname','age'],
                template: '<h1>{{id --- name ---  age}}</h1>
                 <button @click="goRegister">跳转到注册页面</button>'，
                methods: {
                    goRegister( ){
                        this.$router.push('/register')
                    }
                }
            }




    <script>
        const user = {
            template: `<div>
            <h1>user</h1>
            <button @click="goBack">跳转到注册页面</button>
             </div>`,
            methods: {
                goBack: function() {
                    this.$router.go(-1)
                }
            }
        }

        const register = {
            template: `<div>
            <h1>register</h1>
            <hr>
            <!-- 子路由链接 -->
            <router-link to="/register/tab1">tab1</router-link>
            <router-link to="/register/tab2">tab2</router-link>
            <button @click="goRegister">跳转到注册页面</button>
            <!-- 子路由占位符 -->
            <router-view></router-view>
             </div>`,
            methods: {
                goRegister: function() {
                    this.$router.push('/user')
                }
            }
        }

        const tab1= {
            template: `<h1>tab1</h1>`
        }

        const tab2= {
            template: `<h1>tab2</h1>`
        }


        const router = new VueRouter({
            routes: [
                {path:'/',redirect: '/user'},
                {path:'/user',component: user},
                {path:'/register',component: register,children: [
                    {path:'/register/tab1',component: tab1},
                    {path:'/register/tab2',component: tab2}
                ]}
            ]
        })
        
        const vm = new Vue({
            el: '#app',
            data: {
                
            },
            router
        })
    </script>


## VUE - cli
    使用版本为4.5.13
    vue create 项目名称
    vue ui  图形化



## Element-UI
    基于vue的组件库
    
    


## 路由导航守卫
router.beforeEach( (to,from,next) => {
    to:将要访问的路径
    from：从哪里来
    next是一个函数，表示放行 next() 
    next('/url')  强制跳转
})



## vue-x
    实现组件之间数据共享，全局状态管理的一种机制

    

### store.js

    State:提供唯一的公共数据源
    Mutation:用于变更store的数据，可以监控数据的变化  mutation中的方法可以传参  !!! 不要在里面执行异步操作，不如计时器
    Action:处理异步任务  可以传参  !!! 注意action不可以直接操作state的数据，必须通过mutation来操作  
    Getter: 对state中的数据进行包装生成新数据，新数据会随着原数据的变换而变化    
    modules:

#### 访问全局状态的方法

    1.this.$store.state.全局名称

    2.通过mapState函数
        通过vue-x导入mapState
        import { mapState } from 'vuex' 
        在计算属性中引入进来
        computed: {
            ...mapState(['count']) // ...用来展开
        },

#### 调用mutation的方法
    mutations: {
        add (state) {
        state.count++
        },
        subtract (state) {
        state.count--
        }
    }

    第一种方法：
        methods: {
            substract () {
            this.$store.commit('subtract',params)   //commit的作用就是调用一个mutation
            }
        }

    第二种方法：
        通过vue-x导入mapMatutions
        import { mapMatutions } from 'vuex' 
        在methods中引用进来
            methods: {
                ...mapMatutions(['add','subtract'])
            }

#### Action
    actions: {
        addAsync (context[,params]) {   //context相当于store对象
        setTimeout(() => {
            context.commit('add'[,params])
        }, 1000)
        }
    },
    调用action中方法的方法

        第一种：
             methods: {
                add01 () {
                  this.$store.dispatch('addAsync'[,params])   //dispatch函数专门用来出发action
                }
            }


        第二种方法
            通过vue-x导入mapActions
        import { mapActions } from 'vuex' 
        在methods中引用进来
            methods: {
                ...mapActions(['addAsync','addNAsync']),
                click () {
                    this.addAsync()
                }
            }


#### getters
    getters: {
        showNum (state) {
        return '当前数量为' + state.count
        }
    }

    第一种方法：
    <h3>当前最新的count值为：{{$store.getters.showNum}}</h3>

    第二种方法
        导入vue-x的mapGetters
            在计算属性中引用
            computed: {
                ...mapGetters(['count']) // ...用来展开
            },