
## 示例

### 列表的显示

通过输入框的值，模糊查询显示列表

**通过函数实现**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
    <script src="../lib/vue-2.6.14.js"></script>  
</head>  
<body>  
<div id="app">  
    <input v-model="temp"/>  
    <ul>  
        <li v-for="item in list"  
            :key="item.id"  
            v-show="isShow(temp, item.name)">  
            {{item.name}} - {{item.age}}  
        </li>  
    </ul>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    const vm = new Vue({  
        el: "#app",  
        data: {  
            temp: "",  
            list: [  
                {  
                    id: "001",  
                    name: "马冬梅",  
                    age: "18",  
                },  
                {  
                    id: "002",  
                    name: "周冬雨",  
                    age: "14",  
                },  
                {  
                    id: "003",  
                    name: "周杰伦",  
                    age: "20",  
                },  
            ],  
        },  
        methods: {  
            isShow(s, name) {  
                if (!s) {  
                    return true;  
                }  
                return name.indexOf(s) !== -1;  
            }  
        },  
    });  
  
</script>  
</body>  
</html>
```


**通过watch实现**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
    <script src="../lib/vue-2.6.14.js"></script>  
</head>  
<body>  
<div id="app">  
    <input v-model="temp"/>  
    <ul>  
        <li v-for="item in fillList"  
            :key="item.id">  
            {{item.name}} - {{item.age}}  
        </li>  
    </ul>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    const vm = new Vue({  
        el: "#app",  
        data: {  
            temp: "",  
            list: [  
                {id: "001", name: "马冬梅", age: "18"},  
                {id: "002", name: "周冬雨", age: "14"},  
                {id: "003", name: "周杰伦", age: "20"},  
            ],  
            fillList: []  // 过滤的数据  
        },  
        watch: {  
            temp: {  
                immediate: true,  
                handler(newVal) {  
                    this.fillList = this.list.filter((obj) => {  // filter 过滤返回新数组  
                        return obj.name.indexOf(newVal) !== -1;  
                    })  
                }  
            }  
        }  
    });  
  
</script>  
</body>  
</html>
```


**通过计算属性实现（优先使用）**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
    <script src="../lib/vue-2.6.14.js"></script>  
</head>  
<body>  
<div id="app">  
    <input v-model="temp"/>  
    <ul>  
        <li v-for="item in fillList"  
            :key="item.id">  
            {{item.name}} - {{item.age}}  
        </li>  
    </ul>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    const vm = new Vue({  
        el: "#app",  
        data: {  
            temp: "",  
            list: [  
                {id: "001", name: "马冬梅", age: "18"},  
                {id: "002", name: "周冬雨", age: "14"},  
                {id: "003", name: "周杰伦", age: "20"},  
            ],  
        },  
        computed: {  
            fillList() {  
                return this.list.filter((obj) => {  
                    return obj.name.indexOf(this.temp) !== -1;  
                });  
            }  
        }  
    });  
  
</script>  
</body>  
</html>
```

在上面基础上制作升序降序以及默认排序按钮

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
    <script src="../lib/vue-2.6.14.js"></script>  
</head>  
<body>  
<div id="app">  
    <label>  
        <input v-model="temp"/>  
    </label>  
    <button @click="sortType = 0">升</button>  
    <button @click="sortType = 1">降</button>  
    <button @click="sortType = 2">原</button>  
  
    <ul>   <!-- 数据全部来源于 fillList 故关于数据的操作全部在 fillList 中完成 -->
        <li v-for="item in fillList"   
            :key="item.id">  
            {{item.name}} - {{item.age}}  
        </li>  
    </ul>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    new Vue({  
        el: "#app",  
        data: {  
            temp: "",  
            list: [  
                {id: "001", name: "马冬梅", age: "18"},  
                {id: "002", name: "周冬雨", age: "24"},  
                {id: "003", name: "周杰伦", age: "20"},  
                {id: "004", name: "张一山", age: "26"},  
                {id: "005", name: "刘德华", age: "40"},  
            ],  
            sortType: 2,   // 0：升  1：降  2：默认  
        },  
        computed: {  
            fillList: {  
				get() {  
				    let tmp = this.list.filter((obj) => {  
				        return obj.name.indexOf(this.temp) !== -1;  
				    });  
				    switch (this.sortType) {  
				        case 0:  
				            tmp.sort((a, b) => a.age - b.age);  
				            break;  
				        case 1:  
				            tmp.sort((a, b) => b.age - a.age);  
				            break;  
				    }  
				    // 可以用三元代替
					// if (this.sortType !== 2) {  
					//     tmp.sort((o1, o2) => {  
					//         return this.sortType ? o2.age-o1.age : o1.age-o2.age;  
					//     });  
					// }
				    return tmp;  
				},
            }  
        },  
    });  
  
</script>  
</body>  
</html>
```


### v-model 收集表单数据

* `type="text"、"password"、"radio"` 时，获取的是value值，需要配置value
* `type="checkbox"` 时，如果没有配置value，当v-model初始值为非数组获取的是是否勾选bool，如果初始值为数组获取的是勾选的元素的数组

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
    <script src="../lib/vue-2.6.14.js"></script>  
</head>  
<body>  
<div id="app">  
    <form @submit.prevent="submit">  
        <label><span>账号: </span><input type="text" v-model.trim="userInfo.account"/></label>  
        <br><br>  
        <label><span>密码: </span><input type="password" v-model.trim="userInfo.password"/></label>  
        <br><br>  
        <label><span>年龄: </span><input type="number" v-model.number="userInfo.age"/></label>  
        <br><br>  
        <span>性别: </span>  
        <label><span>男</span><input type="radio" name="sex" v-model="userInfo.sex" value="男"/></label>  
        <label><span>女</span><input type="radio" name="sex" v-model="userInfo.sex" value="女"/></label>  
        <br><br>  
        <span>爱好: </span>  
        <label><span>抽烟</span><input type="checkbox" v-model="userInfo.hobby" value="抽烟"/></label>  
        <label><span>喝酒</span><input type="checkbox" v-model="userInfo.hobby" value="喝酒"/></label>  
        <label><span>烫头</span><input type="checkbox" v-model="userInfo.hobby" value="烫头"/></label>  
        <br><br>  
        <span>所属校区: </span>  
        <label>  
            <select v-model="userInfo.city">  
                <option value="">请选择</option>  
                <option value="北京">北京</option>  
                <option value="上海">上海</option>  
                <option value="深圳">深圳</option>  
                <option value="湖北">湖北</option>  
            </select>  
        </label>  
        <br><br>  
        <label>  
            <span>其他</span>  
            <textarea v-model.lazy="userInfo.other"></textarea>  
        </label>  
        <br><br>  
        <label><input type="checkbox" v-model="userInfo.agree"/></label>  
        <span>阅读并接受<a href="">《用户协议》</a></span>  
        <br><br>  
        <button>提交</button>  
    </form>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    const vm = new Vue({  
        el: "#app",  
        data: {  
            userInfo: {  
                account: '',  
                password: '',  
                sex: '',  
                hobby: [],  
                city: '',  
                other: '',  
                agree: false,  
                age: '',  
            }  
        },  
        methods: {  
            submit() {  
                // console.log(JSON.stringify(this._data))   不建议  
                console.log(JSON.stringify(this.userInfo))  
            }  
        }  
    });  
</script>  
</body>  
</html>
```


### Todo列表

组件间通过App.vue通信

**App.vue**

一般来说，不会将列表数据传入`Input`和`Foot`，为了可以在这两个组件实现添加和删除，可以在`App.vue`中定义添加删除的函数，只需要接收删除和添加的元素，在`App.vue`中完成，而子组件只需要传入函数，然后在事件函数中调用父组件传入的函数即可

如果想要数据本地存储，可以给 `TodoList` 添加侦听器（由于更改复选框属于更改数组中对象里的属性，所以需要深度侦听 `deep`），然后更改TodoList从浏览器本地获取，每次更新 `TodoList` 就将数据同步到浏览器

```vue
<template>  
  <div>  
	<Input :TodoList="TodoList"/>  <!-- 可以传入函数 addTodo -->
	<List :TodoList="TodoList"/>  
	<Foot :TodoList="TodoList"/>
  </div>  
</template>  
  
<script>  
import Input from "@/components/Input";  
import List from "@/components/List";  
import Foot from "@/components/Foot";  
  
export default {  
  name: "App",  
  data() {  
    return {  
      TodoList: [  
        {id: 0, msg: "起床", flag: false},  
        {id: 1, msg: "睡觉", flag: false},  
        {id: 2, msg: "吃饭", flag: false},  
      ]  
      // TodoList: JSON.parse(localStorage.getItem('TodoList')) || []
    }  
  },  
  components: {  
    Input,  
    List,  
    Foot  
  },
  //methods: {  
  //addTodo(obj) {  
  //  if (obj) {  
  //    this.TodoList.unshift(obj)  
  //  }  
  //},    
  // watch: {  
  //   TodoList: {  
  //     deep: true,  
  //     handler(value) {  
  //       localStorage.setItem("TodoList", JSON.stringify(value))  
  //     }  
  //   }  
  // }
}  
</script>  
  
<style>  
ul {  
  list-style: none;  
}  
</style>
```


**Input.vue**

id需要唯一标识，可以使用`Date.now()` 获取当前时间戳来作为id，除此之外，可以借助第三方库实现

* `uuid`：通过计算机地址物理地址国家等信息，生成全球唯一字符串标识，但体积很大
* `nanoid`：相当于 `uuid` 的简化版，体积非常小，直接调用 `nanoid()` 即可生成唯一标识

由于数据特殊，Vue无法侦听到对象中元素变化，所以可以采用 `v-model` 修改数据，但是不是很建议

```vue
<template>  
  <input type="txt" @keyup.enter="add" v-model.trim="msg"/> 
</template>  
  
<script>  
import {nanoid} from 'nanoid'

export default {  
  name: "Input",  
  data() {  
    return {  
      msg: ""  
    }  
  },  
  props: {  
    TodoList: {     // 可以不接受列表，改为函数 addTodo
      type: Array,  
      required: true  
    }  
  },  
  methods: {  
    add() {     // 在回调函数中处理好要添加的对象，然后调用 addTodo 即可
      if (this.msg) {  
        this.TodoList.unshift({id: nanoid(), msg: this.msg, flag: false})  
        this.msg = ""
      }  
    },  
  }  
}  
</script>  
  
<style scoped>  
  
</style>
```

**List.vue**

```vue
<template>  
  <div id="List">  
    <ul>  
      <li v-for="item in TodoList" :key="item.id">  
	    <!-- vue无法检测对象里数据的变化 -->
        <input type="checkbox" v-model="item.flag"/>  
        <span>{{item.msg}}</span>  
        <button @click="deleteList(item.id)">Delete</button>  
      </li>  
    </ul>  
  </div>  
</template>  
  
  
<script>  
export default {  
  name: "List",  
  props: {  
    TodoList: {  
      type: Array,  
      required: true  
    }  
  },  
  methods: {  
    deleteList(id) {  
      let index = this.TodoList.findIndex((item) => item.id === id)  
      this.TodoList.splice(index, 1)  
    }  
  }  
}  
</script>  
  
<style scoped>  
  
</style>
```

**Foot.vue**

```vue
<template>  
  <div>  
    <input type="checkbox" v-model="All">  
    <span>已完成{{ count }}/总{{ TodoList.length }}</span>  
    <button @click="clear">清除已完成</button>  
  </div>  
</template>  
  
<script>  
export default {  
  name: "Foot",  
  props: {  
    TodoList: {  
      type: Array,  
      required: true  
    }  
  },  
  methods: {  
    clear() {  
      let arr = this.TodoList.filter(item => !item.flag)  
      let n = this.TodoList.length  
      this.TodoList.splice(0, n)  
  
      arr.forEach(item => {  
        this.TodoList.push(item)  
      })  
    }  
  },  
  computed: {  
    count() {  
      let n = 0  
      this.TodoList.forEach((item) => {  
        n += item.flag ? 1 : 0  
      })  
      return n  
    },  
    All: {  
      get() {  
        if (this.TodoList) {  
          return false  
        }  
        return this.TodoList.every((item) => item.flag)  
      },  
      set(val) {  
        this.TodoList.forEach((item) => {  
          item.flag = val  
        })  
      }  
    }  
  },  
}  
</script>
```

**main.js**

```js
import Vue from 'vue'  
import App from '@/App.vue'  

Vue.config.productionTip = false  

new Vue({  
  render: h => h(App),  
	}).$mount('#app')
```

### Github 用户搜索

借助 api 通过输入框输入的关键词，来搜索与关键词相关的用户，并展示

api：`https://www.api.github.com/search/users?q=xxx`

返回数据详解

```json
{
"total_count": 105677,   // 总共有
  "incomplete_results": false,   // 是否显示所有
  "items": [
    {
      "login": "test",   // 用户名
      "id": 383316,      // 唯一标识 id
      "node_id": "MDQ6VXNlcjM4MzMxNg==",
      "avatar_url": "https://avatars.githubusercontent.com/u/383316?v=4",   // 头像地址
      "gravatar_id": "",
      "url": "https://api.github.com/users/test",   
      "html_url": "https://github.com/test",    // 主页
      "followers_url": "https://api.github.com/users/test/followers",
      "following_url": "https://api.github.com/users/test/following{/other_user}",
      "gists_url": "https://api.github.com/users/test/gists{/gist_id}",
      "starred_url": "https://api.github.com/users/test/starred{/owner}{/repo}",
      "subscriptions_url": "https://api.github.com/users/test/subscriptions",
      "organizations_url": "https://api.github.com/users/test/orgs",
      "repos_url": "https://api.github.com/users/test/repos",
      "events_url": "https://api.github.com/users/test/events{/privacy}",
      "received_events_url": "https://api.github.com/users/test/received_events",
      "type": "User",
      "site_admin": false,
      "score": 1.0
    }]
}    
```