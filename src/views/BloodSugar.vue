<template>
  <div class="page-container">
    <div class="page-header">
      <button class="back-btn" @click="$router.push('/')">←</button>
      <h2 class="page-title">血糖记录</h2>
    </div>

    <div class="form-card">
      <div class="form-group">
        <label>血糖值 (mmol/L)</label>
        <input v-model.number="form.level" type="number" placeholder="请输入血糖值" step="0.1" />
      </div>
      <div class="form-group">
        <label>测量类型</label>
        <select v-model="form.type">
          <option value="">请选择</option>
          <option value="空腹">空腹</option>
          <option value="餐后">餐后</option>
          <option value="随机">随机</option>
        </select>
      </div>
      <div class="form-group">
        <label>测量时间</label>
        <input v-model="form.time" type="datetime-local" />
      </div>
      <button class="submit-btn" @click="handleSubmit">记录</button>
    </div>

    <div class="record-list">
      <h3 class="list-title">记录列表</h3>
      <div v-if="records.length">
        <div class="record-card" v-for="r in records" :key="r.id">
          <div class="record-info">
            <span><span class="highlight">{{ r.level }} mmol/L</span></span>
            <span :class="['result-tag', r.type === '空腹' ? 'result-normal' : r.type === '餐后' ? 'result-overweight' : 'result-underweight']">{{ r.type }}</span>
            <span>{{ r.time }}</span>
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

function getDefaultDateTime() {
  const d = new Date()
  const pad = n => String(n).padStart(2, '0')
  return `${d.getFullYear()}-${pad(d.getMonth() + 1)}-${pad(d.getDate())}T${pad(d.getHours())}:${pad(d.getMinutes())}`
}

const STORE = 'bloodSugar'
const form = ref({ level: null, type: '', time: getDefaultDateTime() })
const records = ref([])

async function handleSubmit() {
  if (!form.value.level || !form.value.type || !form.value.time) return
  await addRecord(STORE, { ...form.value, time: form.value.time.replace('T', ' ') })
  form.value = { level: null, type: '', time: getDefaultDateTime() }
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