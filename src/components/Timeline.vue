<template>
  <div class="timeline-content">
    <h3>时间线</h3>
    <div class="timeline">
      <div class="timeline-item" v-for="(event, index) in timelineEvents" :key="index">
        <div class="timeline-marker"></div>
        <div class="timeline-content">
          <h4>{{ event.year }}</h4>
          <p>{{ event.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const timelineEvents = [
  { year: '19XX年', description: '零号空洞出现，标志着空洞时代的开始' },
  { year: '2030年', description: '乔伊斯建立调查公会ACE，制定空洞探索标准' },
  { year: '2035年', description: '维克上校、丹中校与法金汉佣兵团团长联手将"黑墙"斥退37公里' },
  { year: '2040年', description: '亚契建立赫利俄斯机关，深入研究空洞特性' },
  { year: '2045年', description: '挽昼成功消解天上的"巢主"空洞，发明邦布' },
  { year: '2050年', description: '星见家三代家主斩杀称颂会司教' }
]
</script>

<style scoped>
.timeline-content h3 {
  font-size: 28px;
  margin-bottom: 20px;
  text-align: center;
}

.timeline {
  position: relative;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px 0;
}

.timeline::before {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  width: 4px;
  background: rgba(255, 255, 255, 0.2);
  left: 50%;
  transform: translateX(-50%);
}

.timeline-item {
  display: flex;
  align-items: center;
  margin-bottom: 40px;
  position: relative;
}

.timeline-marker {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: white;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1;
}

.timeline-content {
  width: 45%;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  backdrop-filter: blur(10px);
}

.timeline-item:nth-child(odd) .timeline-content {
  margin-left: auto;
}

.timeline-item:nth-child(even) .timeline-content {
  margin-right: auto;
}

.timeline-content h4 {
  margin-top: 0;
  font-size: 20px;
  color: #fff;
}

.timeline-content p {
  margin-bottom: 0;
  font-size: 16px;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .timeline::before {
    left: 30px;
  }
  
  .timeline-marker {
    left: 30px;
  }
  
  .timeline-content {
    width: calc(100% - 80px);
    margin-left: 80px !important;
    margin-right: 0 !important;
  }
}
</style>