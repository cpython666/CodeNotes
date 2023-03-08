# Vue3+Vite构建步骤

## 用`vite`初始化vue项目(回车)

```js
npm create vite@latest vueVitePro -- --template vue
```

## 安装配置路由vue-router

```js
npm install vue-router@4    

import router from './router/index.js'
createApp(App).use(router).mount('#app')  
```

## 安装 element-plus 及图标

```js
npm install element-plus --save
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
createApp(App).use(ElementPlus ).mount('#app')

npm install @element-plus/icons-vue
使用图标：   <el-icon><Setting /></el-icon>
import {Setting}  from "@element-plus/icons-vue"
components:{  Setting  }
```

## 安装axios并挂载在vue原型上

```js
import axios from 'axios'n'p
app.config.globalProperties.$http= axios
```

## 安装并配置全局事件总线vuex

```js
npm install vuex@next --save

import store from './store/index.js'
createApp(App).use(store).mount('#app')  
```

## 引入echarts

```js
npm install echarts vue-echarts

import ECharts from 'vue-echarts'
import 'echarts'

createApp(App).component('e-charts',ECharts).mount('#app')
```

# 项目构建后项目目录结构及代码

## 项目结构及文件夹作用

`node_modules`:存放安装的依赖包

`public和assets`：均可用来存放图片，视频等静态资源

`router`：存放路由相关配置

`store`：存放全局事件总线相关配置

`components`：存放一些自定义的小型组件

`views`：存放页面组件（每一个页面就是一个组件）

`package-lock.json`：记录项目所用到的依赖和配置

`index.html`：起始页面

`App.vue`：总组件

`main.js`：vue组件的引入注册，配置相关文件

## 项目代码

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + Vue</title>
  </head>
  <body>
  <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

`App.vue`

```vue
<template>
  <div>
  </div>
  <router-view></router-view>
</template>

<style scoped>
</style>
<script>
</script>
```

`main.js`

```js
import { createApp } from 'vue'
import App from './App.vue'

import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

import router from './router/index.js'

import store from './store/index.js'

import axios from 'axios'

import ECharts from 'vue-echarts'
import 'echarts'

const app=createApp(App)

app.use(store).use(router).use(ElementPlus).mount('#app')
app.component('e-charts',ECharts)
app.config.globalProperties.$http= axios
```

`router/index.js`

```js
import Home from '../components/Home.vue'
import Login from '../components/Login.vue'
import StudentList from '../components/StudentList.vue'
import { createRouter,createWebHashHistory} from 'vue-router'
  // 3. 创建路由实例并传递 `routes` 配置
  // 你可以在这里输入更多的配置，但我们在这里
  // 暂时保持简单
  const router = createRouter({
    // 4. 内部提供了 history 模式的实现。为了简单起见，我们在这里使用 hash 模式。
    history: createWebHashHistory(),
    routes: [
      {
        path: '/', 
        component: Login,
      },
      { 
        path: '/home', 
        component: Home,
        children: [
          {
            path: 'studentlist',
            component:StudentList,
          }
        ]
      }
      ], // `routes: routes` 的缩写
  })
  
export default router
```

`store/index.js`

```js
import { createStore } from 'vuex'

// 创建一个新的 store 实例
const store = createStore({
  state () {
    return {
      count: 0
    }
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
export default store
```

> 注意：components文件夹内荣自己定义，因此并未给出。router/index.js文件内组件及引入相关内容需因项目改变。

# 在组件中使用echarts

- 在页面中定义一个dom容器

```html
<template>
   <div id="myChart" :style="{ width: '300px', height: '300px' }"></div>
</template>
```

- 在script中引入使用echarts，定义配置

```js
<script>
  import { defineComponent, toRefs, reactive, onMounted } from 'vue'
  import * as echarts from 'echarts'
  
  export default defineComponent({
    name: 'Histogram',
    setup() {
      const state = reactive({
        option: {
          grid: {
            top: '4%',
            left: '2%',
            right: '4%',
            bottom: '0%',
            containLabel: true,
          },
          xAxis: [
            {
               type: 'category',
               data: ["芳草地国际","实验小学","白家庄小学","外国语小学","师范学校附属","望京东园"],
               axisTick: { alignWithLabel: true },
               axisLabel: { interval: 0, rotate: 30 },
            },
          ],
          yAxis: [
            {
              type: 'value',
            },
          ],
          series: [
            {
              name: '学校',
              type: 'bar',
              barWidth: '40%',
              data: [260,680,360,460,150,320],
            },
          ],
        },
      })
      const initeCharts = () => {
        let myChart = echarts.init(document.getElementById('myChart'))
        // 绘制图表
        myChart.setOption(state.option)
      }
      
      onMounted(() => {
        initeCharts()
      })

      return {
        ...toRefs(state),
      }
    },
  })
</script>
```



# Vue3重要知识点

## 生命周期

```js
2、created -> 使用 setup()	

3、beforeMount -> onBeforeMount

4、mounted -> onMounted	//在渲染完html后执行

5、beforeUpdate -> onBeforeUpdate

6、updated -> onUpdated	//第二次进入页面执行

7、beforeDestroy -> onBeforeUnmount

8、deactivated -> onDeactivated		//退出当前页面

9、errorCaptured -> onErrorCaptured	//浅出Vue 错误处理机制

10、activated ->	onActivated 	//每次都执行

import { onMounted, onUpdated, onUnmounted } from 'vue'
export default {
setup() {
    onMounted(() => {
      console.log('mounted!')
    })
    onUpdated(() => {
      console.log('updated!')
    })
    onUnmounted(() => {
      console.log('unmounted!')
    })
  },
};

onMounted(()=>{

})
```

## 事件

```html
<template>
  <div id='box' class="box">
    <div class="yanjin">
      <div class="demo" ref='seder'>欢迎来到VUE3</div>
      <div class="demo" @click="testClick">欢迎来到VUE3</div>
    </div>
    <div
      v-for="(item,index) in list" :key='index'>
      <p v-if="item.type == 1" @click="setAder">
        {{item.text}}
      </p>
    </div>
    <div @click="go">跳转页面</div>
    <div @click="getRquset">点我调接口</div>
  </div>
</template>
```



```html
<style src='./index.less' lang='less' scoped>
</style>
<script>
import { getFissionCourseList, getGetrequs } from "../../utils/request";
import { reactive, toRefs, onMounted, onActivated } from "vue";
export default {
  components: {},
  setup() {
    const state = reactive({
      testMsg: "原始值",
      list: [
        {
          text: 123,
          type: 1,
        },
        {
          text: 321,
          type: 0,
        },
        {
          text: 427,
          type: 1,
        },
        {
          text: 346,
          type: 0,
        },
      ],
    });
    onMounted(async () => {
      console.log("mounted!");
      // 进入页面调用接口
      await getFissionCourseList({ t35: "post" }).then((res) => {
        console.log(res);
      });
    });
    onActivated(async () => {
      let that = this;
    });
    // 点击事件
    const methods = {
      go() {
        this.$router.push({
          path: "/main",
          query: {
            course_id: 123,
          },
        });
      },
      testClick() {
        let that = this;
        that.$nextTick(function() {
          that.$refs.seder.innerHTML = "更改了内容";
        });
      },
      async getRquset() {
        await getGetrequs({ t35: "get" }).then((res) => {
          console.log(res);
        });
      },
    };
    return {
      ...toRefs(state),
      ...methods,
    };
  },
};
</script>
```

# 实战

## element-plus,登陆注册表单页面

### 登录`LoginPage.vue`

```js
<template>
    <el-row  justify="center">
        <el-col :span="8" height="100vh">
            <el-form :model="form" justify="center">
                <div class="between">
                    <h3>登录</h3>
<el-link @click="turn('resiger')">没有账号?点此注册</el-link>
</div>
<el-form-item>
                    <el-input v-model="form.name" placeholder="手机/邮箱/昵称"/>
                    </el-form-item>
                    <el-form-item>
                    <el-input v-model="form.password" type="password" placeholder="请输入密码"/>
                    </el-form-item>
                    <div class="between">
                    <el-checkbox-group v-model="form.remember">
                    <el-checkbox label="记住我"/>
                    </el-checkbox-group>
                    <el-link>忘记密码?</el-link>
                    </div>
                    <div id="center">
                    <el-button type="primary" @click="onSubmit">登录</el-button>
                    </div>
                    </el-form>
                    </el-col>
                    </el-row>
                    </template>
                    <style scoped>
                    h3{
                margin:0;
}
    .el-row{
        align-items: center;
        height:100vh;
    }
.el-form{
    height:80%;
    border: 1px solid var(--el-border-color);
    padding: 10%;
    max-width: 250px;
}
.between{
    display: flex;
    justify-content: space-between;
    padding-bottom: 10px;
}
#center{
    display:flex;
    justify-content: center;
    width: 100%;
}
</style>


<script lang="ts" setup>
    import { reactive} from 'vue'
import { useRouter } from 'vue-router';
import axios from 'axios'

const router=useRouter()
const turn=(path)=>{
    router.replace(
        {path:`${path}`}
    )
}
const form = reactive({
    name: '',
    password:'',
    remember:[]
})
const onSubmit=()=>  {
    console.log('登陆操作!')
    axios.post(`http://localhost:8080/user/queryuser/${form.name}/${form.password}`).then(function(res){
        if (res.data){
            console.log("成功")

            turn('userdata')
        }else{
            console.log("失败")
        }
    })
}


</script>
```

### 注册`LoginPage.vue`

```js
<template>
    <el-row justify="center">
        <el-col :span="12" height="100vh" id="center">
            <el-form
ref="ruleFormRef"
:model="ruleForm"
:rules="rules"
class="demo-ruleForm"
:size="formSize"
labelWidth="80px"
status-icon
>
    <div class="between">
        <h3>注册</h3>
<el-link @click="turn('login')">已有账号?点此登录</el-link>
</div>
<el-form-item label="姓名" prop="name">
        <el-input v-model="ruleForm.name" placeholder="请输入姓名"/>
        </el-form-item>
        <el-form-item label="性别" prop="sex">
        <el-radio-group v-model="ruleForm.sex">
        <el-radio label="男" />
        <el-radio label="女" />
        </el-radio-group>
        </el-form-item>
        <el-form-item label="出生日期">
        <el-form-item prop="date1">
        <el-date-picker
    v-model="ruleForm.date1"
    type="date"
    placeholder="选择出生日期"
    width="100%"
    />
        </el-form-item>
        </el-form-item>
        <el-form-item label="电话号码" prop="phone">
        <el-input v-model="ruleForm.phone" placeholder="请输入电话号码"/>
        </el-form-item>
        <el-form-item label="账号" prop="account">
        <el-input v-model="ruleForm.account" placeholder="请输入账号"/>
        </el-form-item>
        <el-form-item label="密码" prop="password">
        <el-input v-model="ruleForm.password" type="password" placeholder="请输入密码"/>
        </el-form-item>
        <el-form-item label="密码" prop="password2">
        <el-input v-model="ruleForm.password2" type="password" placeholder="请再次输入密码"/>
        </el-form-item>
        <el-form-item label="个性签名" prop="desc">
        <el-input v-model="ruleForm.desc" type="textarea" />
        </el-form-item>
        <div id="center">
        <el-button type="primary" @click="submitForm(ruleFormRef)"
        >注册</el-button
        >
        <el-button @click="resetForm(ruleFormRef)">重置</el-button>
        </div>
        </el-form>
        </el-col>
        </el-row>
        </template>
        <style scoped>
        /* body{
    overflow-y: hidden;
  } */
        /* el-row{
    height: 100%;
  } */
        h3{
    margin:0;
}
    .el-form-item{
        width: 100%;
    }
.between{
    max-width: 350px;
    display: flex;
    justify-content: space-between;
    padding-bottom: 10px;
    width: 100%;
}
#center{
    display:flex;
    justify-content: center;
    width: 100%;
}
.el-form{
    padding:8%;
    max-width: 350px;
    display: flex;
    flex-direction: column;
    align-items: center;
}
</style>
<script lang="ts" setup>
    import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { useRouter } from 'vue-router';
import axios from 'axios'
const router=useRouter()
const turn=(path)=>{
    router.replace(
        {path:`${path}`}
    )
}
const formSize = ref('default')
const ruleFormRef = ref<FormInstance>()
const ruleForm = reactive({
    name: '',
    sex:'',
    date1: '',
    type: [],
    account: '',
    password: '',
    password2: '',
    desc: '',
    phone:''
})
const validatePass = (rule: any, value: any, callback: any) => {
    if (value === '') {
        callback(new Error('请输入密码'))
    } else {
        if (ruleForm.password2 !== '') {
            if (!ruleFormRef.value) return
            ruleFormRef.value.validateField('checkPass', () => null)
        }
        callback()
    }}

const validatePass2 = (rule: any, value: any, callback: any) => {
    if (value === '') {
        callback(new Error('请再次输入密码'))
    } else if (value !== ruleForm.password) {
        callback(new Error("两次密码不相同"))
    } else {
        callback()
    }
} 
const rules = reactive<FormRules>({
    account: [
        { required: true, message: '请输入账号', trigger: 'blur' },
        { min: 3, max: 11, message: '长度在3到11之间', trigger: 'blur' },
    ],
    phone: [
        { required: true, message: '请输入电话号码', trigger: 'blur' },
        { min: 11, max: 11, message: '长度为11', trigger: 'blur' },
    ],
    password: [
        { min: 3, max: 11, message: '长度在3到11之间', trigger: 'blur' },
        { required:true,validator: validatePass, trigger: 'blur' }
    ],
    password2: [{ required:true,validator: validatePass2, trigger: 'blur' }],
    sex: [
        {
            required: true,
            message: '请选择性别',
            trigger: 'change',
        },
    ],
    date1: [
        {
            type: 'date',
            required: false,
            message: '请选择出生日期',
            trigger: 'change',
        }
    ],
    desc: [
        { required: false, message: '请输入个性签名', trigger: 'blur' },
    ],
})

const submitForm = async (formEl: FormInstance | undefined) => {
    if (!formEl) return
    await formEl.validate((valid, fields) => {
        if (valid) {
            console.log(ruleForm)
            console.log('提交成功!')
            onSubmit()
        } else {
            console.log('格式错误!', fields)
        }
    })
}

const resetForm = (formEl: FormInstance | undefined) => {
    if (!formEl) return
    formEl.resetFields()
}

const onSubmit=()=>  {
    console.log('注册操作!')
    let param = new URLSearchParams()
    param.append('user_name', ruleForm.account)
    param.append('user_password', ruleForm.password)

    // const param=reactive({
    //   'user_name': ruleForm.name,
    //   'user_password': ruleForm.password
    // })

    axios.post(`http://localhost:8080/user/insert`,param).then(function(res){
        if (res.data){
            console.log("成功")
            turn('userdata')
        }else{
            console.log("失败")
        }
    })
}
</script>
```

## 获取近期登录人数

```js
getLoginSum(){
    var _this=this
    axios.get('http://localhost:8080/userlog/queryAllLog').then(function (res) {
        _this.loginSum = res.data.length
        console.log(_this.loginSum)
        var date,year,mouth,day,date2

        var todayLoginSum=0,yLoginSum=0,yyLoginSum=0,yyyLoginSum=0,yyyyLoginSum=0,yyyyyLoginSum=0

        var now=new Date()

        year=now.getFullYear()
        mouth=now.getMonth()+1
        day=now.getDate()
        console.log(year,mouth,day)
        var date1=new Date(year,mouth,day)
        date1=date1.getTime()
        console.log(date1)

        for(var i=0; i<res.data.length;i++){
            date=res.data[i].login_time
            year=date.slice(0,4)
            mouth=date.slice(5,7)
            day=date.slice(8,10)
            date2=new Date(year,mouth,day)
            date2=date2.getTime()

            // console.log(date2)

            if(date1==date2){
                todayLoginSum++
                console.log('今天')
            }else if(date1-date2==24*60*60*1000){
                yLoginSum++
                console.log('一天前')
            }else if(date1-date2==24*60*60*1000*2){
                yyLoginSum++
                console.log('两天前')
            }else if(date1-date2==24*60*60*1000*3){
                console.log('三天前')
                yyyLoginSum++
            }else if(date1-date2==24*60*60*1000*4){
                yyyyLoginSum++
                console.log('四天前')
            }else if(date1-date2==24*60*60*1000*5){
                yyyyyLoginSum++
                console.log('五天前')
            }
        }
        _this.todayLoginSum = todayLoginSum 
        _this.historyLoginSum=[todayLoginSum,yLoginSum,yyLoginSum,yyyLoginSum,yyyyLoginSum, yyyyyLoginSum]
        _this.historyLoginSum.reverse()

    })
}
```

## 时间显示组件

```js
<template>
    <div>
    {{dateYear}} {{dateWeek}} {{dateDay}}
</div>
</template>

<script>
   import dayjs from "dayjs"
export default {
    data () {
        return {
            dateDay: null,
            dateYear: null,
            dateWeek: null,
            weekday: ["周日", "周一", "周二", "周三", "周四", "周五", "周六"],
            timer:null
        };
    },
    mounted () {
        this.timer =setInterval(() => {
            const date=dayjs(new Date())
            this.dateDay = date.format('HH:mm:ss');
            this.dateYear = date.format('YYYY-MM-DD');
            this.dateWeek = date.format(this.weekday[date.day()]);

        }, 1000)
    },
    beforeDestroy(){
        if(this.timer){
            clearInterval(this.timer)
        }
    }
};
</script>
```



