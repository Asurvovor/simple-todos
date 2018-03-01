<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <ul class="todos">
        <li>
            <input v-model="newTodo"
                   @keyup.13="addItem"
                   placeholder="快写下您要我记住的事吧"
                   autofocus="ture" />
        </li>
        <li v-for="(todo,index) in todos" 
        :id="index" 
        :class="{'checked': todo.done}">
            <input type="checkbox"
                   @change="saveToStore"
                   v-model="todo.done" />
            <label>{{ index + 1 }}.{{ todo.value }}</label>
            <time>{{ todo.created | date }}</time>
            <button @click.prevent="delItem(todo)"></button>
        </li>
    </ul>

<!-- 
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul> 
-->
  </div>
</template>

<script>
import './assets/site.less'
import moment from 'moment'
import 'moment/locale/zh-cn'
moment.locale('zh-cn')
export default {
    name: 'app',
    data () {
        return {
            // msg: 'Welcome to Your Vue.js App'
            newTodo: '',
            title: "vue-todos",
            todos: [],
            // todos: [
            //     { 
            //         value: "阅读一本关于前端开发的书", 
            //         done: false,
            //         created: Date.now(),
            //     },
            //     { 
            //         value: "补充范例代码", 
            //         done: true,
            //         created: Date.now() + 300000,
            //     },
            //     { 
            //         value: "写心得",
            //         done: false,
            //         created: Date.now() - 30000000
            //     },
            // ],
        }
    },
    created () {
        if(this.is_initialized) {
            this.todos = JSON.parse(localStorage.getItem('VUE-TODOS'))
        }
    },
    computed: {
        is_initialized () {
            return localStorage.getItem('VUE-TODOS') != null
        }
    },
    filters: {
        date(val) {
            return moment(val).calendar()
        }
    },
    methods: {
        addItem() {
            this.todos.push({
                value: this.newTodo,
                created: Date.now(),
                done: false,
            })
            this.saveToStore()
            this.newTodo = ''
        },
        delItem(todo) {
            this.todos = this.todos.filter((x) => x !== todo)
            this.saveToStore()
        },
        saveToStore() {
            localStorage.setItem('VUE-TODOS', JSON.stringify(this.todos))
        },
    },
}
</script>

<style lang="less" scoped>
@import './assets/todos.less';
/*#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}*/
</style>
