<template>
  <div class="disasters-content">
    <h3>空洞灾厄</h3>
    <div class="disasters-grid">
      <div class="disaster-card" v-for="(disaster, index) in disasters" :key="index">
        <h4>{{ disaster.name }}</h4>
        <p>{{ disaster.description }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const disasters = [
  { name: '零号空洞', description: '第一个出现的空洞，也是最神秘的空洞，亚契进入后尚未归还' },
  { name: '黑墙', description: '一种具有侵蚀性的空洞现象，曾被维克等人成功斥退' },
  { name: '巢主', description: '天空中的巨大空洞，被挽昼成功消解' },
  { name: '神的迷宫', description: '由乔伊斯只身穿越的神秘空洞，为后续探索建立了标准方案' }
]
</script>

<style scoped>
.disasters-content h3 {
  font-size: 28px;
  margin-bottom: 20px;
  text-align: center;
}

.disasters-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.disaster-card {
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  padding: 20px;
  backdrop-filter: blur(10px);
  transition: transform 0.3s ease;
}

.disaster-card:hover {
  transform: translateY(-5px);
}

.disaster-card h4 {
  margin-top: 0;
  font-size: 20px;
  color: #fff;
}

.disaster-card p {
  font-size: 16px;
  margin-bottom: 0;
}
</style>