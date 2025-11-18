<template>
  <div class="flex items-center justify-between px-4 py-3 bg-white/80 backdrop-blur-md rounded-2xl shadow-lg">
    <span class="text-slate-600">
      总计：<strong class="text-slate-800">{{ count }}</strong>
    </span>
    <div class="flex gap-2">
      <button
        v-for="(label, idx) in ['全部', '未完成', '已完成']"
        :key="idx"
        @click="$emit('changeTabType', idx)"
        :class="[
          'px-4 py-2 rounded-lg text-sm font-medium transition-all',
          tabType === idx 
            ? 'bg-blue-500 text-white shadow-md' 
            : 'bg-slate-100 text-slate-600 hover:bg-slate-200'
        ]"
      >
        {{ label }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoFooter',
  props: ['count', 'tabType']
}
</script>