<template>
  <div class="page-container">
    <div class="page-header">
      <button class="back-btn" @click="$router.push('/')">←</button>
      <h2 class="page-title">步数记录</h2>
    </div>

    <div class="form-card">
      <div class="form-group">
        <label>步数</label>
        <input v-model.number="form.steps" type="number" placeholder="请输入步数" />
      </div>
      <div class="form-group">
        <label>日期</label>
        <input v-model="form.date" type="date" />
      </div>
      <button class="submit-btn" @click="handleSubmit">记录</button>
    </div>

    <div class="record-list">
      <h3 class="list-title">记录列表</h3>
      <div v-if="records.length">
        <div class="record-card" v-for="r in records" :key="r.id">
          <div class="record-info">
            <span><span class="highlight">{{ r.date }}</span></span>
            <span><span class="highlight">{{ r.steps.toLocaleString() }} 步</span></span>
          </div>
          <button class="delete-btn" @click="handleDelete(r.id)">删除</button>
        </div>
      </div>
      <div v-else class="empty-text">暂无记录</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { addRecord, getAllRecords, deleteRecord } from '../db'

function getDefaultDate() {
  const d = new Date()
  const pad = n => String(n).padStart(2, '0')
  return `${d.getFullYear()}-${pad(d.getMonth() + 1)}-${pad(d.getDate())}`
}

const STORE = 'steps'
const form = ref({ steps: null, date: getDefaultDate() })
const records = ref([])

async function handleSubmit() {
  if (!form.value.steps || !form.value.date) return
  await addRecord(STORE, { ...form.value })
  form.value = { steps: null, date: getDefaultDate() }
  await loadRecords()
}

async function loadRecords() {
  records.value = await getAllRecords(STORE)
}

async function handleDelete(id) {
  await deleteRecord(STORE, id)
  await loadRecords()
}

function formatTime(s) {
  return new Date(s).toLocaleString('zh-CN')
}

onMounted(loadRecords)
</script>