<template>
  <div class="tdFooter">
    <span>总计：{{ count }}</span>
    <div class="tdTabs">
      <a
        :class="{active:tabType == 0}"
        @click="tabClick(0)"
      >全部</a>
      <a
        :class="{active:tabType == 1}"
        @click="tabClick(1)"
      >未完成</a>
      <a
        :class="{active:tabType == 2}"
        @click="tabClick(2)"
      >已完成</a>
    </div>
  </div>
</template>

<script>
export default {
  props: ['count', 'tabType'],
  methods: {
    tabClick(newTabtype) {
      this.$emit('changeTabType', newTabtype)
    }
  }
}
</script>

<style>
.footer {
  color: #777;
  padding: 10px 15px;
  height: 58px;
  text-align: center;
  border-top: 1px solid #e6e6e6;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.todo-count {
  float: left;
  text-align: left;
}
.filters {
  margin: 0;
  padding: 0;
  list-style: none;
  display: flex;
  gap: 8px;
}
.filters li a {
  color: inherit;
  text-decoration: none;
  padding: 3px 7px;
  border: 1px solid transparent;
  border-radius: 3px;
  cursor: pointer;
}
.filters li a.selected {
  border-color: rgba(175, 47, 47, 0.2);
}
.filters li a:hover {
  border-color: rgba(175, 47, 47, 0.1);
}
</style>