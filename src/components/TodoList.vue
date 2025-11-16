<template>
  <div class="tdContainer">
    <ul class="tdList">
      <li
        class="tdItem"
        v-for="item in todos"
        :key="item.id"
      >
        <div class="tdItem-main">
          <input
            type="checkbox"
            v-model="item.completed"
            class="toToggle"
          />
          <span
            class="tdTxt"
            :class="{completed:item.completed}"
          >
            {{ item.txt }}
          </span>
        </div>
        <div class="meta">
          <span class="date">{{ fmtDate(item.createdAt) }}</span>
          <span v-if="item.priority===1" class="badge">重要</span>
          <span v-if="item.priority===2" class="badge urgent">紧急</span>
          <span v-if="item.pinned" class="pin">📌</span>
        </div>
        <div class="tdItem-acts">
          <a @click="$emit('togglePin', item)" class="pin-btn">
            {{ item.pinned ? '取消置顶' : '置顶' }}
          </a>
          <a @click="delTodo(item)">删除</a>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['todos'],
  methods: {
    delTodo(item) {
      this.$emit('delTodo', item)
    },
    fmtDate(iso) {
      return new Date(iso).toLocaleString('zh-CN', {
        month: 'numeric',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      })
    }
  }
}
</script>

<style scoped>
.todo-list {
  margin: 0;
  padding: 0;
  list-style: none;
}
.todo-item {
  position: relative;
  font-size: 18px;
  border-bottom: 1px solid #ededed;
  padding: 16px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.todo-item.pinned {
  background: #fff9e6;
  border-left: 4px solid #ffa500;
}
.view {
  flex: 1;
  display: flex;
  align-items: center;
  flex-wrap: wrap;
}
.toggle {
  width: 20px;
  height: 20px;
  margin-right: 12px;
}
label {
  flex: 1;
  word-break: break-word;
}
label.completed {
  text-decoration: line-through;
  color: #d9d9d9;
}
.meta {
  width: 100%;
  margin-left: 32px;
  margin-top: 6px;
  font-size: 12px;
  color: #999;
}
.badge {
  padding: 2px 6px;
  border-radius: 3px;
  margin-left: 8px;
  font-size: 11px;
  font-weight: bold;
}
.badge.important {
  background: #e6f7ff;
  color: #1890ff;
}
.badge.urgent {
  background: #fff1f0;
  color: #ff4d4f;
}
.pin {
  margin-left: 8px;
}
.actions {
  display: flex;
  gap: 8px;
}
.pin-btn, .destroy {
  padding: 4px 8px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  font-size: 12px;
}
.pin-btn {
  background: #52c41a;
  color: white;
}
.destroy {
  background: #ff4d4f;
  color: white;
}
</style>