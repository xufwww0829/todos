<template>
  <transition-group name="slide" tag="ul" class="space-y-3">
    <li
      v-for="item in todos"
      :key="item.id"
      :class="[
        'task-card bg-white/90 backdrop-blur-md rounded-xl p-4 shadow-md border-l-4',
        item.pinned ? 'bg-gradient-to-r from-yellow-50 to-amber-50 border-amber-400' : 'border-transparent',
        item.priority === 1 ? 'border-l-blue-400' : '',
        item.priority === 2 ? 'border-l-red-400' : '',
        item.completed ? 'opacity-60' : ''
      ]"
    >
      <div class="flex items-center justify-between">
        <div class="flex items-start gap-3 flex-1">
          <input
            type="checkbox"
            v-model="item.completed"
            class="w-6 h-6 mt-1 cursor-pointer rounded"
          />
          <div class="flex-1">
            <div 
              :class="[
                'text-lg font-medium',
                item.completed ? 'line-through text-slate-400' : 'text-slate-800'
              ]"
            >
              {{ item.txt }}
            </div>
            <div class="flex items-center gap-2 mt-2 text-sm text-slate-500">
              <span>{{ fmtDate(item.createdAt) }}</span>
              <span 
                v-if="item.priority === 1"
                class="px-2 py-1 bg-blue-100 text-blue-700 rounded-full text-xs font-medium"
              >
                重要
              </span>
              <span 
                v-if="item.priority === 2"
                class="px-2 py-1 bg-red-100 text-red-700 rounded-full text-xs font-medium"
              >
                紧急
              </span>
              <span v-if="item.pinned" class="text-yellow-600">📌</span>
            </div>
          </div>
        </div>
        <div class="flex gap-2">
          <button
            @click="$emit('togglePin', item)"
            :class="[
              'px-3 py-1 rounded-lg text-xs font-medium transition-all',
              item.pinned 
                ? 'bg-yellow-100 text-yellow-700 hover:bg-yellow-200' 
                : 'bg-slate-100 text-slate-600 hover:bg-slate-200'
            ]"
          >
            {{ item.pinned ? '取消置顶' : '置顶' }}
          </button>
          <button
            @click="delTodo(item)"
            class="px-3 py-1 bg-red-100 text-red-700 rounded-lg text-xs font-medium hover:bg-red-200 transition-all"
          >
            删除
          </button>
        </div>
      </div>
    </li>
  </transition-group>
</template>

<script>
export default {
  name: 'TodoList',
  props: ['todos'],
  methods: {
    delTodo(item) {
      this.$emit('delTodo', item)
    },
    fmtDate(iso) {
      const d = new Date(iso)
      return `${d.getMonth() + 1}月${d.getDate()}日 ${d.getHours().toString().padStart(2, '0')}:${d.getMinutes().toString().padStart(2, '0')}`
    }
  }
}
</script>

<style scoped>
.slide-enter-active, .slide-leave-active {
  transition: all 0.5s ease;
}
.slide-enter-from {
  opacity: 0;
  transform: translateY(20px);
}
.slide-leave-to {
  opacity: 0;
  transform: translateX(-100px);
}
</style>