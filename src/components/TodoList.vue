<template>
  <ul class="todo-list">
    <li 
      v-for="item in sortedTodos"
      :key="item.id"
      class="todo-item"
      :class="{ 'pinned': item.isPinned, 'urgent': item.priority === 'urgent', 'critical': item.priority === 'critical' }"
      @mouseenter="hoverId = item.id"
      @mouseleave="hoverId = null"
    >
      <button 
        v-show="hoverId === item.id"
        class="pin-btn"
        @click="togglePin(item)"
      >
        {{ item.isPinned ? '取消置顶' : '置顶' }}
      </button>
      <input type="checkbox" v-model="item.completed" />
      <span :class="{ completed: item.completed }">{{ item.txt }}</span>
      <span class="date">{{ formatDate(item.createdAt) }}</span>
      <span class="priority" :class="item.priority">{{ getPriorityText(item.priority) }}</span>
      <a
        v-show="hoverId === item.id"
        class="destroy"
        @click="$emit('delTodo', item)"
      >
        删除
      </a>
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue'
const props = defineProps({ todos: Array })
defineEmits(['delTodo'])
const hoverId = ref(null)

// 添加排序逻辑
const sortedTodos = computed(() => {
  return [...props.todos].sort((a, b) => {
    if (a.isPinned !== b.isPinned) return b.isPinned - a.isPinned
    if (a.priority !== b.priority) {
      const priorityOrder = { critical: 3, urgent: 2, normal: 1 }
      return priorityOrder[b.priority] - priorityOrder[a.priority]
    }
    return new Date(b.createdAt) - new Date(a.createdAt)
  })
})

// 添加辅助方法
function formatDate(date) {
  return new Date(date).toLocaleDateString()
}

function getPriorityText(priority) {
  const map = { normal: '普通', urgent: '紧急', critical: '非常紧急' }
  return map[priority]
}

function togglePin(item) {
  item.isPinned = !item.isPinned
}

</script>

<style scoped>
.todo-item {
  padding: 10px 20px;
  border-bottom: 1px solid #ddd;
  display: flex;
  align-items: center;
  gap: 10px;
}

.pinned {
  background-color: #fff9e6;
}

.urgent {
  border-left: 3px solid #ff9800;
}

.critical {
  border-left: 3px solid #f44336;
}

.pin-btn {
  padding: 2px 8px;
  font-size: 12px;
  background: #4e6ef2;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.date {
  font-size: 12px;
  color: #999;
}

.priority {
  font-size: 12px;
  padding: 2px 6px;
  border-radius: 4px;
}

.priority.normal {
  background: #e8f5e9;
  color: #4caf50;
}

.priority.urgent {
  background: #fff3e0;
  color: #ff9800;
}

.priority.critical {
  background: #ffebee;
  color: #f44336;
}

</style>
