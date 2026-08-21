<template>
  <div class="page-container">
    <div class="page-header">
      <button class="back-btn" @click="$router.push('/')">←</button>
      <h2 class="page-title">血压记录</h2>
    </div>

    <div class="form-card">
      <div class="form-group">
        <label>姓名</label>
        <input v-model="form.name" type="text" placeholder="请输入姓名" />
      </div>
      <div class="form-group">
        <label>收缩压 (mmHg)</label>
        <input v-model.number="form.systolic" type="number" placeholder="高压" />
      </div>
      <div class="form-group">
        <label>舒张压 (mmHg)</label>
        <input v-model.number="form.diastolic" type="number" placeholder="低压" />
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
            <span><span class="highlight">{{ r.name }}</span></span>
            <span>收缩压 <span class="highlight">{{ r.systolic }}</span></span>
            <span>舒张压 <span class="highlight">{{ r.diastolic }}</span></span>
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

const STORE = 'bloodPressure'
const form = ref({ name: '', systolic: null, diastolic: null, time: '' })
const records = ref([])

async function handleSubmit() {
  if (!form.value.name || !form.value.systolic || !form.value.diastolic || !form.value.time) return
  await addRecord(STORE, { ...form.value })
  form.value = { name: '', systolic: null, diastolic: null, time: '' }
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