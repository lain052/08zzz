<template>
  <div class="factions-content">
    <h3>势力团体</h3>
    <div class="factions-grid">
      <div class="faction-card" v-for="(faction, index) in factions" :key="index">
        <h4>{{ faction.name }}</h4>
        <p>{{ faction.description }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const factions = [
  { name: '调查公会ACE', description: '由乔伊斯创立，专门负责空洞探索和研究的组织，是调查员和绳匠的先祖' },
  { name: '节杖军', description: '一支军事化组织，致力于对抗空洞威胁，保护人类安全' },
  { name: '赫利俄斯机关', description: '由亚契创立的科研机构，专门研究空洞的特性和规律' },
  { name: '玛瑟尔集团', description: '一家大型企业，参与空洞研究并开发相关技术产品' },
  { name: '星见家族', description: '拥有悠久历史的家族，以剑术闻名，在对抗空洞势力中发挥重要作用' },
  { name: '法金汉佣兵团', description: '专业的佣兵组织，在与空洞的战斗中表现出色' },
  { name: '梅弗劳尔家族', description: '最初的艾利都建城家族' }
]
</script>

<style scoped>
.factions-content h3 {
  font-size: 28px;
  margin-bottom: 20px;
  text-align: center;
}

.factions-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.faction-card {
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  padding: 20px;
  backdrop-filter: blur(10px);
  transition: transform 0.3s ease;
}

.faction-card:hover {
  transform: translateY(-5px);
}

.faction-card h4 {
  margin-top: 0;
  font-size: 20px;
  color: #fff;
}

.faction-card p {
  font-size: 16px;
  margin-bottom: 0;
}
</style>