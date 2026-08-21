<template>
  <div class="home-container">
    <header class="home-header">
      <h1>生活健康小助手</h1>
      <p class="subtitle">记录每一天，守护健康生活</p>
    </header>
    <div class="feature-grid">
      <div class="feature-card" @click="$router.push('/bmi')">
        <div class="card-icon">⚖️</div>
        <div class="card-name">BMI 计算</div>
      </div>
      <div class="feature-card" @click="$router.push('/water')">
        <div class="card-icon">💧</div>
        <div class="card-name">饮水建议</div>
      </div>
      <div class="feature-card" @click="$router.push('/medicine')">
        <div class="card-icon">💊</div>
        <div class="card-name">用药记录</div>
      </div>
      <div class="feature-card" @click="$router.push('/blood-pressure')">
        <div class="card-icon">🩺</div>
        <div class="card-name">血压记录</div>
      </div>
      <div class="feature-card" @click="$router.push('/blood-sugar')">
        <div class="card-icon">🩸</div>
        <div class="card-name">血糖记录</div>
      </div>
      <div class="feature-card" @click="$router.push('/heart-rate')">
        <div class="card-icon">❤️</div>
        <div class="card-name">心率记录</div>
      </div>
      <div class="feature-card" @click="$router.push('/sleep')">
        <div class="card-icon">🌙</div>
        <div class="card-name">睡眠记录</div>
      </div>
      <div class="feature-card" @click="$router.push('/steps')">
        <div class="card-icon">🚶</div>
        <div class="card-name">步数记录</div>
      </div>
    </div>
  </div>
</template>

<script setup>
</script>

<style scoped>
.home-container {
  padding: 24px 16px;
}

.home-header {
  text-align: center;
  margin-bottom: 28px;
}

.home-header h1 {
  font-size: 22px;
  color: #2c3e50;
  margin-bottom: 4px;
}

.subtitle {
  color: #aaa;
  font-size: 13px;
}

.feature-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
}

.feature-card {
  background: #fff;
  border-radius: 14px;
  padding: 20px 14px;
  text-align: center;
  cursor: pointer;
  box-shadow: 0 1px 6px rgba(0,0,0,0.05);
  transition: transform 0.15s;
}

.feature-card:active {
  transform: scale(0.96);
}

.card-icon {
  font-size: 32px;
  margin-bottom: 8px;
}

.card-name {
  font-size: 14px;
  font-weight: 600;
  color: #2c3e50;
}
</style>