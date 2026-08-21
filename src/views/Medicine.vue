<template>
  <div class="page-container">
    <div class="page-header">
      <button class="back-btn" @click="$router.push('/')">←</button>
      <h2 class="page-title">用药记录</h2>
    </div>

    <div class="form-card">
      <div class="form-group">
        <label>药名</label>
        <input v-model="form.name" type="text" placeholder="请输入药品名称" />
      </div>
      <div class="form-group">
        <label>药量</label>
        <input v-model="form.dosage" type="text" placeholder="如：2片 / 5ml" />
      </div>
      <div class="form-group">
        <label>用药时间</label>
        <input v-model="form.time" type="datetime-local" />
      </div>
      <button class="submit-btn" @click="handleSubmit">记录</button>
    </div>

    <div class="record-list">
      <h3 class="list-title">记录列表</h3>
      <div v-if="records.length">
        <div class="record-card" v-for="r in records" :key="r.id">
          <div class="record-info">
            <span>药名 <span class="highlight">{{ r.name }}</span></span>
            <span>药量 <span class="highlight">{{ r.dosage }}</span></span>
            <span>时间 <span class="highlight">{{ r.time }}</span></span>
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

const STORE = 'medicine'
const form = ref({ name: '', dosage: '', time: '' })
const records = ref([])

async function handleSubmit() {
  if (!form.value.name || !form.value.dosage || !form.value.time) return
  await addRecord(STORE, { ...form.value })
  form.value = { name: '', dosage: '', time: '' }
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