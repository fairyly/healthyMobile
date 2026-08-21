<template>
  <div class="page-container">
    <div class="page-header">
      <button class="back-btn" @click="$router.push('/')">←</button>
      <h2 class="page-title">心率记录</h2>
    </div>

    <div class="form-card">
      <div class="form-group">
        <label>心率 (次/分钟)</label>
        <input v-model.number="form.bpm" type="number" placeholder="请输入心率" />
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
            <span><span class="highlight">{{ r.bpm }} bpm</span></span>
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

const STORE = 'heartRate'
const form = ref({ bpm: null, time: '' })
const records = ref([])

async function handleSubmit() {
  if (!form.value.bpm || !form.value.time) return
  await addRecord(STORE, { ...form.value })
  form.value = { bpm: null, time: '' }
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