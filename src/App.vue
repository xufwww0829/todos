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

<style >
html,
body {
  background-color: #f5f5f5;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  max-width: 980px;

  min-height: 100%;
  margin: 0 auto;
}
</style>