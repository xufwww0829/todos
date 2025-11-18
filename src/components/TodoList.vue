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
.tdList {
    list-style: none;
    padding: 0;
    text-align: left;
    background-color: #fff;
    border-radius: 10px;
}

.tdItem {
    padding: 10px 20px 10px 10px;
    border-bottom: 1px solid #ddd;
    cursor: pointer;
    display: flex;
    justify-content: space-between;
}

.tdItem:last-child {
    border-bottom: 0;
}

.tdToggle {
    cursor: pointer;
}

.tdTxt {
    padding-left: 10px;
}

.completed {
    text-decoration: line-through;
    color: #999;
}

.tdItem-acts {
    display: none;
    color: red;
}

.tdItem:hover .tdItem-acts {
    display: block;
}
</style>
