<template>
  <footer v-show="count !== undefined" class="footer">
    <span>总计：{{ count }}</span>
    <div class="filters">
      <a :class="{ active: tabType === 0 }" @click="$emit('changeTabType', 0)">全部</a>
      <a :class="{ active: tabType === 1 }" @click="$emit('changeTabType', 1)">未完成</a>
      <a :class="{ active: tabType === 2 }" @click="$emit('changeTabType', 2)">已完成</a>
    </div>
  </footer>
</template>

<script setup>
defineProps({
  count: Number,
  tabType: { type: Number, default: 0 }
})
defineEmits(['changeTabType'])
</script>

<style scoped>
.footer { display:flex; justify-content:space-between; margin-top:10px; font-size:14px; }
.filters a { margin-left:8px; cursor:pointer; }
.filters a.active { font-weight:bold; color:#000; }
</style>