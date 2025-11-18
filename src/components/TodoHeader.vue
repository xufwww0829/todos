<template>
  <div class="space-y-3">
    <div class="flex flex-col md:flex-row gap-4">
      <input
        v-model.trim="newTodo"
        @keyup.enter="addNewTodo"
        maxlength="50"
        placeholder="请输入您的待办事项，按下回车后即可添加"
        class="flex-1 px-4 py-3 border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent text-lg"
      />
      <select 
        v-model="priority"
        class="px-4 py-3 border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 bg-white"
      >
        <option :value="0">普通</option>
        <option :value="1">重要</option>
        <option :value="2">紧急</option>
      </select>
      <button
        @click="addNewTodo"
        class="add-btn px-6 py-3 bg-gradient-to-r from-blue-500 to-blue-600 text-white rounded-lg font-medium transition-all hover:scale-105 hover:from-blue-600 hover:to-blue-700 shadow-md"
      >
        添加任务
      </button>
    </div>
    <div class="flex items-center">
      <span 
        v-show="newTodo.length > 0"
        class="text-sm text-slate-500"
      >
        {{ newTodo.length }}/50
      </span>
      <span 
        v-show="newTodo.length >= 50"
        class="ml-2 text-sm text-red-600 font-medium"
      >
        最多输入 50 个字符哟！！！
      </span>
    </div>
  </div>
</template>

<script>
const WORD_COUNT_LIMIT = 50
export default {
  name: 'TodoHeader',
  data() {
    return {
      newTodo: '',
      countLimit: WORD_COUNT_LIMIT,
      priority: 0
    }
  },
  methods: {
    addNewTodo() {
      if (this.newTodo.length === 0 || this.newTodo.length > WORD_COUNT_LIMIT) return
      this.$emit('addTodo', { txt: this.newTodo, priority: this.priority })
      this.newTodo = ''
      this.priority = 0
    }
  }
}
</script>