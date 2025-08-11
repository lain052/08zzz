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
  { name: '空洞调查协会-HIA', description: '由乔伊斯创立，专门负责空洞探索和研究的组织。空洞调查协会是由新艾利都官方授权的空洞行动组织。主要工作是吸纳、培训、管理空洞调查员,处理官方认可的空洞事务委托,灾情数据记录,核发报酬等。' },
  { name: '节杖军', description: '一支军事化组织，致力于对抗空洞威胁，保护人类安全' },
  { name: '赫利俄斯机关', description: '由亚契创立的科研机构，专门研究空洞的特性和规律' },
  { name: '玛瑟尔集团', description: '一家大型企业，参与空洞研究并开发相关技术产品' },
  { name: '星见家族', description: '拥有悠久历史的家族，以剑术闻名，在对抗空洞势力中发挥重要作用' },
  { name: '法金汉佣兵团', description: '专业的佣兵组织，在与空洞的战斗中表现出色' },
  { name: '梅弗劳尔家族', description: '最初的艾利都建城家族' },
  { name: '绳匠', description: '指引空洞路径之人.' },
  { name: '怀斯塔学会', description: '致力于空洞研究的学术组织，汇聚了众多学者和研究人员。' },
  { name: '防卫军', description: '一支由空洞专家组成的军事组织，负责对抗空洞威胁。' },
  { name: '称颂会', description: '信仰“始主”的组织，为达成目的不择手段。' },
  { name: '新艾利都治安局-NEPS', description: '由新艾利都官方授权的治安机构，负责对抗空洞威胁。打击违法犯罪，空洞安全管制，疏散受灾市民，提供搜救等。' },
  { name: '对空洞事务特别行动部', description: 'HAND直属武装力量，专职应对空洞灾害。团队拥有最尖端的设备，最强大的队员，也面对最恐怖任务' },
  { name: 'HAND', description: '尚未揭示的神秘势力，可能与空洞调查有关。' },
  { name: '流浪者集团', description: '之前比利曾待过' },
  { name: '律法院', description: '' },
  { name: 'TOPS财联', description: '顶层商业组织，由多家巨头企业联合创办，专门从事空洞研究、技术开发、商业推广等业务。高志，圣霖，乔氏，三门' },
  { name: '', description: '' },


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