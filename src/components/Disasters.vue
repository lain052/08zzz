<template>
  <div class="disasters-content">
    <h3>空洞灾厄</h3>

    <h4>主要空洞</h4>
    <div class="disasters-grid">
      <div class="disaster-card" v-for="(disaster, index) in mainDisasters" :key="index">
        <h4>{{ disaster.name }}</h4>
        <p>{{ disaster.description }}</p>
      </div>
    </div>

    <h4>六大空洞</h4>
    <div class="disasters-grid">
      <div class="disaster-card" v-for="(disaster, index) in sixMajorCavities" :key="index">
        <h4>{{ disaster.name }}</h4>
        <p>{{ disaster.description }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const mainDisasters = [
  { name: '零号空洞-灵簿', description: '第一个出现的空洞，也是最神秘的空洞，亚契进入后尚未归还' },
  { name: '黑墙', description: '一种具有侵蚀性的空洞现象，曾被维克等人成功斥退' },
  { name: '巢主', description: '天空中的巨大空洞，被挽昼成功消解' },
  { name: '神的迷宫', description: '由乔伊斯只身穿越的神秘空洞，为后续探索建立了标准方案' }
]

const sixMajorCavities = [
  { name: '克里特', description: '六大空洞之一，与新艾利都朝夕相伴' },
  { name: '莱姆尼安', description: '六大空洞之一，规模惊人无法被剿灭' },
  { name: '珀塞纳斯', description: '六大空洞之一，与城市共生的威胁' },
  { name: '哈瓦瑞', description: '六大空洞之一，持续产生伴生空洞' },
  { name: '索洛夫', description: '六大空洞之一，对城市安全构成潜在威胁' },
  { name: '帕帕戈', description: '六大空洞之一，象征着人类的脆弱与卑微' }
]
</script>

<style scoped>
.disasters-content h3 {
  font-size: 28px;
  margin-bottom: 20px;
  text-align: center;
}

.disasters-content h4 {
  font-size: 22px;
  margin: 30px 0 15px 0;
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