<template>
  <section class="todoapp">
    <TodoHeader @addTodo="addTodo" />
    <TodoList :todos="filteredTodos" @delTodo="delTodo" />
    <TodoFooter
      :count="filteredTodos.length"
      :tabType="tabType"
      @changeTabType="tabType = $event"
    />
  </section>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

/* -------- 数据 -------- */
const tabType = ref(0)          // 0 全部 1 未完成 2 已完成
const todos = ref(loadTodos())  // 核心数组

/* -------- 计算属性 -------- */
const filteredTodos = computed(() => {
  if (tabType.value === 0) return todos.value
  if (tabType.value === 1) return todos.value.filter(t => !t.completed)
  return todos.value.filter(t => t.completed)
})

/* -------- 方法 -------- */
function addTodo (txt) {
  todos.value.push({ 
    id: Date.now(), 
    txt, 
    completed: false,
    createdAt: new Date().toISOString(),  // 添加创建日期
    priority: 'normal',                  // 添加紧急程度：normal/urgent/critical
    isPinned: false                      // 添加置顶标记
  })
}

function delTodo (item) {
  todos.value = todos.value.filter(t => t.id !== item.id)
}

/* -------- 本地存储 -------- */
watch(todos, () => {
  localStorage.setItem('vue-todos', JSON.stringify(todos.value))
}, { deep: true })

function loadTodos () {
  try {
    return JSON.parse(localStorage.getItem('vue-todos')) || []
  } catch {
    return []
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