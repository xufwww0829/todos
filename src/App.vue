<template>
  <div>
    <TodoHeader @addTodo="addTodo"></TodoHeader>
    <TodoList
      :todos="todoList"
      @delTodo="delTodo"
      @togglePin="togglePin"
    ></TodoList>
    <TodoFooter
      :tabType="tabType"
      :count="todoList.length"
      @changeTabType="changeTabType"
    ></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  components: {
    TodoHeader,
    TodoList,
    TodoFooter
  },
  data() {
    return {
      // 测试数据（带新字段）
      todos: JSON.parse(localStorage.getItem('vue-todos') || '[]').map(t => ({
        ...t,
        createdAt: t.createdAt || new Date().toISOString(),
        priority: t.priority ?? 0,
        pinned: t.pinned ?? false
      })),
      tabType: 0 // 0全部 1未完成 2已完成
    }
  },
  computed: {
    todoList() {
      const tabType = this.tabType
      let arr = this.todos
      if (tabType === 1) arr = arr.filter(t => !t.completed)
      if (tabType === 2) arr = arr.filter(t => t.completed)
      // 置顶→紧急→时间
      return [...arr].sort((a, b) => {
        if (a.pinned && !b.pinned) return -1
        if (!a.pinned && b.pinned) return 1
        if (a.priority !== b.priority) return b.priority - a.priority
        return new Date(b.createdAt) - new Date(a.createdAt)
      })
    }
  },
  watch: {
    todos: {
      handler(todos) {
        localStorage.setItem('vue-todos', JSON.stringify(todos))
      },
      deep: true
    }
  },
  methods: {
    addTodo(payload) {
      const { txt, priority = 0 } = payload
      this.todos.push({
        id: Date.now(),
        txt,
        completed: false,
        createdAt: new Date().toISOString(),
        priority,
        pinned: false
      })
    },
    delTodo(delTodo) {
      this.todos = this.todos.filter(function (item) {
        return item.id !== delTodo.id
      })
    },
    togglePin(item) {
      item.pinned = !item.pinned
    },
    changeTabType(type) {
      this.tabType = type
    }
  }
}
</script>

<style>
/* 全局基础样式 */
* {
  box-sizing: border-box;
}
body {
  font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
  line-height: 1.4em;
  background: #f5f5f5;
  color: #4d4d4d;
  margin: 0;
  padding: 0;
  font-weight: 300;
}
.todoapp {
  background: #fff;
  margin: 130px 0 40px 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}
.todoapp .header h1 {
  position: absolute;
  top: -125px;
  width: 100%;
  font-size: 60px;
  font-weight: 100;
  text-align: center;
  color: rgba(175, 47, 47, 0.15);
  margin: 0;
  padding: 0;
}
.main {
  position: relative;
  z-index: 2;
  border-top: 1px solid #e6e6e6;
}
</style>