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
  { year: '未知', description: '零号空洞出现，标志着空洞时代的开始。它是新艾利都及周遭区域数个大空洞的源头，结构复杂、环境险恶。' },
  { year: '未知', description: '乔伊斯建立调查公会ACE，制定空洞探索标准' },
  { year: '未知', description: '维克上校、丹中校与法金汉佣兵团团长联手将"黑墙"斥退37公里' },
  { year: '未知', description: '亚契建立赫利俄斯机关，深入研究空洞特性' },
  { year: '未知', description: '挽昼成功消解天上的"巢主"空洞，发明邦布' },
  { year: '未知', description: '新艾利都当局确认除零号空洞外，还有六个被重点监控的"大空洞"：克里特、莱姆尼安、珀塞纳斯、哈瓦瑞、索洛夫和帕帕戈。它们与城市朝夕相伴，无声地嘲示着人类的脆弱与卑微。' },
  { year: '未知', description: '星见家三代家主斩杀称颂会司教' },
  { year: '未知', description: '时任艾利都市长表彰七人，赋予虚狩称号' },
  { year: '0', description: '零号空洞的活性在短时间内呈指数级增 长,空洞规模急剧扩张,迅速吞没了艾利都当时最繁华的普鲁托区与密涅瓦区大面积土地。灾害发 生后,艾利都当局投入海量资源进行救灾,努力遏制零号空洞扩张,然而收效甚微,防卫军的伤亡 尤为惨重。 万般无奈之下,时任艾利都领导层决定引爆艾利都西南至东北方向的14座式奥塔。这场爆炸制造了 一条横贯城市的巨大裂谷,但零号空洞可以侵蚀转化的物质陡然减少,空洞活性增强得到了扼制。时 任 艾 利 都 领 导 层 决 定 引 爆 艾 利 都 西 南 至 东 北 方 向 的 14 座 式 舆 塔 。 这 场 爆 炸 制 造 了 一条横贯城市的巨大裂谷,但零号空洞可以侵蚀转化的物质陡然减少,空洞活性增强得到了扼制。 然而在零号空洞的规模和活性都恢复正常后,艾利都已经化为一片废墟,无数人流离失所… 后来,当幸存者在废墟之上擦干眼泪,这座城市更名为新艾利都后,这场巨大的灾难被正式命名 为一一旧都陷落。' },
  { year: '11', description: '至今' }
]
</script>

<style scoped>
.timeline-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.timeline-content h3 {
  font-size: 28px;
  margin-bottom: 20px;
  text-align: center;
}

.timeline {
  position: relative;
  max-width: 800px;
  width: 100%;
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
  width: 100%;
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

.timeline-content .timeline-content {
  width: 45%;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  backdrop-filter: blur(10px);
  margin: 0;
}

.timeline-item:nth-child(odd) .timeline-content {
  margin-left: auto;
}

.timeline-item:nth-child(even) .timeline-content {
  margin-right: auto;
}

.timeline-content .timeline-content h4 {
  margin-top: 0;
  font-size: 20px;
  color: #fff;
}

.timeline-content .timeline-content p {
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

  .timeline-content .timeline-content {
    width: calc(100% - 80px);
    margin-left: 80px !important;
    margin-right: 0 !important;
  }
}
</style>