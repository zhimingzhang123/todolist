<template>
  <section class="real-app">
    <!--keyup 是键盘事件, enter 敲击 Enter按键触发-->
    <!--autofocus自动获取事假的焦点-->
    <input
      type="text"
      class="add-input"
      autofocus="'autofocus"
      placeholder="接下去要做什么?"
      @keyup.enter="addTodo"
    >
    <Item
      :todo="todo"
      v-for="todo of filteredTodos"
      :key="todo.id"
      @del="deleteTodo"
    ></Item>
    <Tabs
      :filter="filter"
      :todos="todos"
      @toggle="toggleFilter"
      @clearAllCompleted="clearAllCompleted"
    >

    </Tabs>
  </section>
</template>

<script>

import Item from './item'
import Tabs from './tabs'

let id = 0
export default {
  name: '',
  data() {
    return {
      todos: [],
      filter: 'all'
    }
  },
  components: {
    Item,
    Tabs,
  },
  methods: {
    addTodo(e){
      this.todos.unshift({ // unshift 插入在列表的第一位
        id: id++,
        content: e.target.value.trim(),  // 去除两边的空格
        completed: false
      })
      e.target.value = ''
    },
    deleteTodo(id) {
      this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1)
    },
    toggleFilter(state) {
      this.filter = state
    },
    clearAllCompleted() {
      // 没有使用splice， 直接对数据进行了重新赋值，拿到未完成的数据
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  },
  computed: {
    // 按照选项去展示对应的数据
    filteredTodos() {
      if(this.filter === 'all') {
        return this.todos
      }
      // 展示completed or active
      const completed = this.filter === 'completed'
      return this.todos.filter(todo => completed === todo.completed)
    }
  }
}
</script>

<style lang="stylus" scoped>
.real-app{
  width 600px
  margin 0 auto
  box-shadow 0 0 5px #666
}
.add-input{
  position: relative;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  border: 0;
  outline: none;
  color: inherit;
  padding: 6px;
  border: 1px solid #999;
  box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
  box-sizing: border-box;
  font-smoothing: antialiased;
  padding: 16px 16px 16px 60px;
  border: none;
  box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
}
/*.tab-container*/
/*background-color #fff*/
/*padding 0 15px*/
</style>
