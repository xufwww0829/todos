<template>
  <header class="header">
    <h1>待办事项</h1>
    <input
      class="new-todo"
      placeholder="请输入您的待办事项，回车后即可添加"
      maxlength="50"
      v-model.trim="newTodo"
      @keyup.enter="handleAdd"
    />
    <select v-model="priority" class="priority-select">
      <option value="normal">普通</option>
      <option value="urgent">紧急</option>
      <option value="critical">非常紧急</option>
    </select>
    <p v-show="newTodo.length >= 50" class="warn">
      最多输入 50 个字符哟！！！
    </p>
  </header>
</template>


<script setup>
import { ref } from 'vue'
const emit = defineEmits(['addTodo'])
const newTodo = ref('')
const priority = ref('normal')  // 添加紧急程度状态

function handleAdd () {
  if (!newTodo.value || newTodo.value.length > 50) return
  emit('addTodo', { text: newTodo.value, priority: priority.value })
  newTodo.value = ''
  priority.value = 'normal'
}

</script>

<style scoped>
.hdContainer {
  text-align: center;
  font-size: 16px;
}

.hdTitle {
  color: #4e6ef2;
}

.newTodo {
  width: 100%;
  padding: 20px 20px;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  box-sizing: border-box;
}

.newTodo:focus {
  outline-color: #4e6ef2;
}

.hdMsg {
  color: red;
  margin: 10px 0;
}
</style>