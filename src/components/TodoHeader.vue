<template>
  <div>
    <h1>待办事项</h1>
    <input
      placeholder="请输入您的待办事项，按下回车后即可添加"
      v-model.trim="newTodo"
      @keyup.enter="addNewTodo"
    />
    <select v-model="priority">
      <option :value="0">普通</option>
      <option :value="1">重要</option>
      <option :value="2">紧急</option>
    </select>
    <p v-show="isShowMsg">
      最多输入{{ countLimit }}个字符！！！
    </p>
  </div>
</template>

<script>
const WORD_COUNT_LIMIT = 50
export default {
  data() {
    return {
      newTodo: '',
      countLimit: WORD_COUNT_LIMIT,
      priority: 0 // 新增
    }
  },
  computed: {
    isShowMsg() {
      return this.newTodo.length > WORD_COUNT_LIMIT
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

<style scoped>
.new-todo {
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
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.priority-select {
  width: 100%;
  padding: 8px;
  font-size: 16px;
  margin-top: 8px;
  border: 1px solid #ddd;
}
.warn {
  color: #e74c3c;
  font-size: 12px;
  margin: 8px 0 0 6px;
  font-weight: bold;
}
</style>