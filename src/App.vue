<template>
  <section class="min-h-screen bg-gradient-to-br from-slate-50 to-slate-200">
    <!-- 导航栏 -->
    <nav class="bg-white/80 backdrop-blur-md shadow-sm sticky top-0 z-50">
      <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center h-16">
          <div class="flex items-center space-x-2">
            <div class="w-8 h-8 bg-gradient-to-br from-blue-500 to-blue-600 rounded-lg flex items-center justify-center">
              <span class="text-white font-bold text-sm">✓</span>
            </div>
            <span class="font-serif text-xl font-bold text-slate-800">智能待办</span>
          </div>
          <div class="flex space-x-6">
            <a href="#" class="text-blue-600 font-medium border-b-2 border-blue-600 pb-1">首页</a>
            <a href="#" class="text-slate-600 hover:text-blue-600 transition-colors">统计</a>
            <a href="#" class="text-slate-600 hover:text-blue-600 transition-colors">关于</a>
          </div>
        </div>
      </div>
    </nav>

    <!-- 主要内容 -->
    <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <!-- 标题 -->
      <div class="text-center mb-8">
        <h1 class="font-serif text-4xl md:text-5xl font-bold text-slate-800 mb-4">
          智能待办事项管理
        </h1>
        <p class="text-lg text-slate-600 max-w-2xl mx-auto">
          高效管理您的日常任务，提升工作效率，让生活更有条理
        </p>
      </div>

      <!-- 统计卡片 -->
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
        <div class="bg-white/80 backdrop-blur-md rounded-xl p-4 text-center shadow-lg">
          <div class="text-2xl font-bold text-slate-800">{{ todos.length }}</div>
          <div class="text-sm text-slate-600">总任务</div>
        </div>
        <div class="bg-white/80 backdrop-blur-md rounded-xl p-4 text-center shadow-lg">
          <div class="text-2xl font-bold text-green-600">{{ completedCount }}</div>
          <div class="text-sm text-slate-600">已完成</div>
        </div>
        <div class="bg-white/80 backdrop-blur-md rounded-xl p-4 text-center shadow-lg">
          <div class="text-2xl font-bold text-orange-600">{{ pendingCount }}</div>
          <div class="text-sm text-slate-600">待完成</div>
        </div>
        <div class="bg-white/80 backdrop-blur-md rounded-xl p-4 text-center shadow-lg">
          <div class="text-2xl font-bold text-blue-600">{{ completionRate }}%</div>
          <div class="text-sm text-slate-600">完成率</div>
        </div>
      </div>

      <!-- 输入区域 -->
      <div class="bg-white/80 backdrop-blur-md rounded-2xl p-6 mb-6 shadow-lg">
        <TodoHeader @addTodo="addTodo" />
      </div>

      <!-- 筛选工具栏 -->
      <div class="bg-white/80 backdrop-blur-md rounded-2xl p-4 mb-6 shadow-lg">
        <div class="flex flex-col md:flex-row justify-between items-center gap-4">
          <div class="flex flex-wrap gap-2">
            <button 
              @click="tabType = 0"
              :class="['filter-btn px-4 py-2 rounded-lg border transition-all', 
                       tabType === 0 ? 'bg-blue-500 text-white border-blue-500' : 'border-slate-200']"
            >
              全部 <span class="ml-1 text-xs rounded-full px-2 py-1 bg-slate-200">{{ todos.length }}</span>
            </button>
            <button 
              @click="tabType = 1"
              :class="['filter-btn px-4 py-2 rounded-lg border transition-all', 
                       tabType === 1 ? 'bg-blue-500 text-white border-blue-500' : 'border-slate-200']"
            >
              未完成 <span class="ml-1 text-xs rounded-full px-2 py-1 bg-orange-200">{{ pendingCount }}</span>
            </button>
            <button 
              @click="tabType = 2"
              :class="['filter-btn px-4 py-2 rounded-lg border transition-all', 
                       tabType === 2 ? 'bg-blue-500 text-white border-blue-500' : 'border-slate-200']"
            >
              已完成 <span class="ml-1 text-xs rounded-full px-2 py-1 bg-green-200">{{ completedCount }}</span>
            </button>
          </div>
          <button 
            @click="clearCompleted"
            class="px-4 py-2 text-red-600 border border-red-200 rounded-lg hover:bg-red-50 transition-colors"
          >
            清除已完成
          </button>
        </div>
      </div>

      <!-- 任务列表 -->
      <div class="space-y-3 mb-8" v-if="todoList.length > 0">
        <TodoList 
          :todos="todoList" 
          @delTodo="delTodo"
          @togglePin="togglePin"
        />
      </div>

      <!-- 空状态 -->
      <div v-else class="text-center py-12">
        <div class="w-24 h-24 mx-auto mb-4 bg-slate-100 rounded-full flex items-center justify-center">
          <span class="text-4xl">📝</span>
        </div>
        <h3 class="text-xl font-semibold text-slate-700 mb-2">暂无任务</h3>
        <p class="text-slate-500">添加您的第一个任务开始高效工作吧！</p>
      </div>

      <!-- 数据操作 -->
      <div class="flex justify-center space-x-4">
        <button 
          @click="exportData"
          class="px-6 py-3 bg-slate-600 text-white rounded-lg hover:bg-slate-700 transition-transform hover:scale-105"
        >
          导出数据
        </button>
        <button 
          @click="$refs.fileInput.click()"
          class="px-6 py-3 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-transform hover:scale-105"
        >
          导入数据
        </button>
        <button 
          @click="clearAll"
          class="px-6 py-3 bg-red-600 text-white rounded-lg hover:bg-red-700 transition-transform hover:scale-105"
        >
          清空所有
        </button>
        <input type="file" ref="fileInput" @change="importData" accept=".json" class="hidden">
      </div>
    </div>

    <!-- 页脚 -->
    <footer class="bg-slate-800 text-white py-8 mt-16">
      <div class="max-w-6xl mx-auto px-4 text-center">
        <p class="text-slate-300">© 2025 智能待办事项管理系统. 让工作更高效，让生活更美好.</p>
      </div>
    </footer>
  </section>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoList
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('vue-todos') || '[]').map(t => ({
        ...t,
        createdAt: t.createdAt || new Date().toISOString(),
        priority: t.priority ?? 0,
        pinned: t.pinned ?? false
      })),
      tabType: 0
    }
  },
  computed: {
    todoList() {
      const tabType = this.tabType
      let arr = this.todos
      if (tabType === 1) arr = arr.filter(t => !t.completed)
      if (tabType === 2) arr = arr.filter(t => t.completed)
      return [...arr].sort((a, b) => {
        if (a.pinned && !b.pinned) return -1
        if (!a.pinned && b.pinned) return 1
        if (a.priority !== b.priority) return b.priority - a.priority
        return new Date(b.createdAt) - new Date(a.createdAt)
      })
    },
    completedCount() {
      return this.todos.filter(t => t.completed).length
    },
    pendingCount() {
      return this.todos.filter(t => !t.completed).length
    },
    completionRate() {
      return this.todos.length ? Math.round((this.completedCount / this.todos.length) * 100) : 0
    }
  },
  watch: {
    todos: {
      handler(todos) {
        localStorage.setItem('vue-todos', JSON.stringify(todos))
      },
      deep: true
    }
  },
  methods: {
    addTodo(payload) {
      const { txt, priority = 0 } = payload
      this.todos.push({
        id: Date.now(),
        txt,
        completed: false,
        createdAt: new Date().toISOString(),
        priority,
        pinned: false
      })
    },
    delTodo(delTodo) {
      this.todos = this.todos.filter(item => item.id !== delTodo.id)
    },
    togglePin(item) {
      item.pinned = !item.pinned
    },
    changeTabType(type) {
      this.tabType = type
    },
    clearCompleted() {
      this.todos = this.todos.filter(t => !t.completed)
    },
    exportData() {
      const data = JSON.stringify(this.todos, null, 2)
      const blob = new Blob([data], { type: 'application/json' })
      const url = URL.createObjectURL(blob)
      const a = document.createElement('a')
      a.href = url
      a.download = `todos_${new Date().toISOString().split('T')[0]}.json`
      a.click()
      URL.revokeObjectURL(url)
    },
    importData(event) {
      const file = event.target.files[0]
      if (!file) return
      const reader = new FileReader()
      reader.onload = (e) => {
        try {
          const data = JSON.parse(e.target.result)
          this.todos = data.map(t => ({
            ...t,
            createdAt: t.createdAt || new Date().toISOString(),
            priority: t.priority ?? 0,
            pinned: t.pinned ?? false
          }))
        } catch (err) {
          alert('文件格式错误！')
        }
      }
      reader.readAsText(file)
      event.target.value = ''
    },
    clearAll() {
      if (confirm('确定清空所有任务？')) {
        this.todos = []
      }
    }
  }
}
</script>