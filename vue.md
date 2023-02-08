<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>vue</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>

    <style>
        #red{
            color: red;
        }
        .active{
            background-color: red;
        }
        .text{
            text-align: center;
            line-height: 200px;
        }
        .green{
            color: green;
        }
    </style>

</head>
<body>

    <!-- 插值渲染 -->
    <div id="main">{{name}}

        <!-- 插值渲染，文本插值 -->
        <p>using 插值：{{rawHtml}}</p>

        <!-- v-html   把取得属性的值（由字符串）转化为纯html格式，渲染到页面 -->
        <p v-html="rawHtml"></p>


        <!-- v-bind:  动态绑定属性：值 对 -->
        <div v-bind:id="color">{{msg}}</div>
        <div v-bind:class="color"></div>
        <a v-bind:href="url">跳转</a>

        <!-- 动态绑定class属性。静态class也能生效，动态绑定一次可绑定多个值 -->
        <div style="width: 200px;height: 200px;"
             class="text"
             v-bind:class="{active : isActive,green:isGreen}">hi Vue</div>

        <!-- v-bind:class 绑定的  数组  写法 -->
        <!-- v-bind:class="[active,green]"       这属于静态绑定 -->
        <!-- v-bind:class="[isActive ? 'active' : '',isGreen ? 'green' : '']"      数组写法-动态绑定 -->



        <!-- vue支持javascript运算插值 -->

        <!-- 简单运算 -->
        <p>{{ number + 1 }}</p>

        <!-- 三元运算 -->
        <p>{{ok ? "yes" : "no"}}</p>

        <!-- 复杂JavaScript函数操作 -->
        <p>{{ message.split('').reverse().join('') }}</p>

    </div>

    <hr>

    <!-- 条件渲染 -->
    <div id="ifs">

          <p v-if="seen">现在你看得到我了</p>


        <div v-if="type ==='A'">A</div>
        <div v-else-if="type === 'B'">B</div>
        <div v-else-if="type === 'C'">C</div>
        <div v-else>not A/B/C</div>

        <!-- 条件渲染——是否显示 -->
        <h1 v-show="ok">hello Vue</h1>

        <!-- v-if  和 v-show 比较。 -->
        <!-- v-if惰性渲染，如果条件不符合，不会渲染；；；v-show条件不符合也会渲染出dom实例，只是加了display=“none”属性 -->
        <!-- v-if    切换开销 -->
        <!-- v-show   初始化开销 -->
    </div>

    <hr>


    <!-- 列表渲染 -->
    <div id="list">
        <ul>
            <!-- 列表渲染，当传过来的是一个数组参数  -->
            <!-- 当传参为数组时，有一个可选参数index，用于返回当前循环的索引 -->

            <!-- 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性： -->
            <li v-for="item,index in items" :key="index">{{index}}{{item.msg}}</li>
        </ul>

        <ul>
            <!-- 列表渲染，当传过来的是一个对象 -->
            <!-- 当传参为对象事，有一个可选参数为key，用于返回当前值得键名 -->

            <!-- 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性： -->
            <li v-for="value,key in objects" :key="key">{{key}}:{{value}}</li>
        </ul>
    </div>



    <hr>




    <!-- 事件处理 -->

    <!-- v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。 -->
    <div id="eventDw">
        <div class="example_1">
            <!-- 将JavaScript代码直接写在v-on绑定中 -->
            <button v-on:click="counter += 1">数值：{{counter}}</button>
        </div>

        <div class="example_2">
            <!-- 处理复杂的事务逻辑，v-on 还可以接收一个需要调用的方法名称 -->
            <button v-on:click="greet">Greet</button>
        </div>

     <!-- 事件处理方法绑定 -->
        <div @click="click1">点击一
            <div @click="click2.stop">点击二</div>
        </div>
    </div>


    <hr>

    <!-- 表单输入绑定 -->
    <div class="formB">
        <!-- v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件： -->
        <!-- 简言之：在类型为text的input和textarea上获得输入框的value属性 -->
        <!-- 文本 -->
        <input type="text" v-model="message">
        <p>message is:{{message}}</p>
        <!-- 多行文本 -->
        <textarea name="" id="" cols="30" rows="10" v-model="message2" placeholder="请输入多行文本"></textarea>
        <p>message2 is:{{message2}}</p>

        <!-- 复选框 -->
        <!-- 多个复选框，绑定到同一个数组： -->
        <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
        <label for="jack">Jack</label>
        <input type="checkbox" id="john" value="John" v-model="checkedNames">
        <label for="john">John</label>
        <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
        <label for="mike">Mike</label>
        <br>
        <span>Checked names: {{ checkedNames }}</span>

        <br>
        <button @click="submit">提交</button>

    </div>


    <hr>



    <!-- vue 组件 -->
    <div id="compon">
        <btn_counter title="title1">
            <h2>hi..h2</h2>
        </btn_counter>
        <btn_counter title="title2"></btn_counter>
        <test></test>
        <test></test>
        <test></test>
        <test></test>
    </div>


    <!-- vue单文件组件 -->
    <!-- 前置环境搭建 -->


    <!-- 1 安装npm
    npm全称为  node Package Manager（node包管理器），是一个机遇node.js的包管理器。
    npm -v 命令查看npm版本

    2 由于网络原因，建议安装cnpm
    npm install -g cnpm --registry=https://registry.npm.taobao.org

    3 安装 vue-cli
    cnpm install -g @vue/cli

    4 安装webpack
    cnpm install -g webpack
    webpack是 JavaScript 打包器（module bundler） -->


    <script>

        // 插值渲染
        // 一般定义一个变量vm，构建实例，vm为view moudle缩写
        var vm = new Vue({
            el:"#main",
            data:{
                msg:"hello world!",
                name:"Vue",
                rawHtml:'<span style="color:red">this should be red</span>',
                color:'red',
                number:10,
                ok:true,
                message:"vue",
                url:"http://www.baidu.com",
                isActive:true,
                isGreen:true
            }
        });



        // 条件渲染
        var ifs = new Vue({
            el:'#ifs',
            data:{
                type:'A',
                seen:true,
                ok:true
            }
        })



        // 列表渲染
        var list = new Vue({
            el:"#list",
            data:{
                items:[
                    {msg:'foo'},
                    {msg:'bar'}
                ],
                objects:{
                    title:'how to learn vue',
                    author:'Ivan',
                    published:'2020-10.1'
                }
            }
        })


        // 事件处理
        var eventDw = new Vue({
            el:"#eventDw",
            data:{
                counter:0,
                name:Vue
            },
            methods:{
                click1:function(){
                    console.log('click1');
                },
                click2:function(){
                    console.log('click2');
                },
                greet:function(){
                    alert('hello' + '!');
                    alert(this.name);
                }
            },

            // 生命周期函数
            // 实例创建之前
            beforeCreate:function(){
                console.log('beforeCreate');
            },
            // 实例创建成功，回调函数
            created:function(){
                console.log('created');
            }
        })



        // 表单数据双向绑定
        var formB = new Vue({
            el:".formB",
            data:{
                message:"",
                message2:"",
                checkedNames:[]       //接收复选框参数用数组
            },
            methods:{
                submit:function(){
                    var postobj = {
                        msg1:this.message,
                        msg2:this.message2
                    };
                    $.ajax({
                        url:"test.php",
                        type:"post",
                        data:JSON.stringify(postobj)
                    })
                }
            }
        })



        // 注册一个vue 组件
        // vue组件 全局注册       试了一下，全局注册要在包含它的vue实例之前声明，不然出不来
        Vue.component('btn_counter',{
            props:['title'],      //给组件定义一个属性
            data:function(){
                return {
                    counter:0
                }
            },
            // slot为插槽，可在页面引用组件时在对应位置插入dom元素
            template:'<div><h1>hi...</h1><button v-on:click="counter ++">{{title}}你点了我{{counter}}次。</button><slot></slot></div>'
        })

        // vue 组件父元素  ：一个vue实例
        var compon = new Vue({
            el:"#compon",
            data:{

            },
            // Vue组件 局部注册
            components:{
                test:{
                    template:"<h2>vue组件局部注册</h2>"
                }
            }
        });






    </script>

</body>
</html>
