<script setup>
import { ref, computed } from 'vue'
import { Capacitor } from '@capacitor/core'

const count = ref(0)
const platform = Capacitor.getPlatform()
const isNative = Capacitor.isNativePlatform()
const now = ref(new Date().toLocaleString('zh-CN'))
setInterval(() => (now.value = new Date().toLocaleString('zh-CN')), 1000)

const todos = ref([{ text: '创建 Vue 项目', done: true }, { text: '导出 IPA', done: false }])
const input = ref('')
const left = computed(() => todos.value.filter(t => !t.done).length)
function add() {
  if (!input.value.trim()) return
  todos.value.push({ text: input.value.trim(), done: false })
  input.value = ''
}
</script>

<template>
  <main class="wrap">
    <h1>Vue iOS 测试</h1>
    <p class="meta">运行平台：<b>{{ platform }}</b>（{{ isNative ? '原生 App' : '浏览器' }}）</p>
    <p class="meta">{{ now }}</p>

    <section class="card">
      <h2>计数器</h2>
      <div class="row">
        <button @click="count--">−</button>
        <span class="num">{{ count }}</span>
        <button @click="count++">＋</button>
      </div>
    </section>

    <section class="card">
      <h2>待办（剩余 {{ left }}）</h2>
      <form class="row" @submit.prevent="add">
        <input v-model="input" placeholder="添加一项…" />
        <button type="submit">添加</button>
      </form>
      <ul>
        <li v-for="(t, i) in todos" :key="i" :class="{ done: t.done }" @click="t.done = !t.done">
          {{ t.done ? '✅' : '⬜️' }} {{ t.text }}
        </li>
      </ul>
    </section>
  </main>
</template>

<style>
* { box-sizing: border-box; }
body { margin: 0; font-family: -apple-system, system-ui, sans-serif; background: #f2f2f7; color: #1c1c1e; }
.wrap { padding: calc(env(safe-area-inset-top) + 16px) 16px 32px; max-width: 520px; margin: 0 auto; }
h1 { font-size: 28px; margin: 8px 0; }
h2 { font-size: 17px; margin: 0 0 12px; }
.meta { color: #6e6e73; margin: 4px 0; font-size: 14px; }
.card { background: #fff; border-radius: 14px; padding: 16px; margin-top: 16px; }
.row { display: flex; gap: 10px; align-items: center; }
.num { font-size: 32px; min-width: 60px; text-align: center; font-variant-numeric: tabular-nums; }
button { background: #42b883; color: #fff; border: 0; border-radius: 10px; padding: 10px 16px; font-size: 16px; }
input { flex: 1; padding: 10px; border: 1px solid #d1d1d6; border-radius: 10px; font-size: 16px; }
ul { list-style: none; padding: 0; margin: 12px 0 0; }
li { padding: 10px 0; border-bottom: 1px solid #eee; }
li.done { color: #8e8e93; text-decoration: line-through; }
</style>
