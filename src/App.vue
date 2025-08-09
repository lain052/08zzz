<script setup>
import welcome from './components/welcome.vue'
import { ref, onMounted, onBeforeUnmount } from 'vue'

// 加载状态
const isAppLoaded = ref(false)
const spriteInterval = ref(null)
const spriteFrame = ref(0)
const totalSpriteFrames = 30 // 3*10精灵图总帧数

// 开始精灵图动画
const startSpriteAnimation = () => {
  spriteInterval.value = setInterval(() => {
    spriteFrame.value = (spriteFrame.value + 1) % totalSpriteFrames
    updateSpriteBackground()
  }, 20)
}

// 停止精灵图动画
const stopSpriteAnimation = () => {
  if (spriteInterval.value) {
    clearInterval(spriteInterval.value)
    spriteInterval.value = null
  }
}

// 更新精灵图背景位置
const updateSpriteBackground = () => {
  const loader = document.querySelector('.sprite-loader')
  if (loader) {
    // 精灵图规格：3列*10行 = 30帧
    const cols = 3
    const rows = 10

    // 每帧尺寸：1590/3 = 530px 宽，1500/10 = 150px 高
    const frameWidth = 530
    const frameHeight = 150

    // 计算当前帧位置
    const row = Math.floor(spriteFrame.value / cols)
    const col = spriteFrame.value % cols

    // 计算背景定位
    const xPos = col * frameWidth
    const yPos = row * frameHeight

    loader.style.backgroundPosition = `-${xPos}px -${yPos}px`
  }
}

// 初始化应用
const initializeApp = () => {

  setTimeout(() => {
    isAppLoaded.value = true
    stopSpriteAnimation()
  }, 5000)
}

onMounted(() => {
  startSpriteAnimation()
  initializeApp()
})

onBeforeUnmount(() => {
  stopSpriteAnimation()
})
</script>

<template>
  <!-- 全局加载动画 -->
  <div v-if="!isAppLoaded" class="app-loading">
    <div class="loading-content">
      <div class="sprite-loader"></div>
    </div>
  </div>

  <!-- 应用主内容 -->
  <div v-show="isAppLoaded">
    <welcome />
  </div>
</template>

<style>
.app-loading {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.loading-content {
  text-align: center;
  color: white;
}

.loading-content p {
  margin-top: 30px;
  font-size: 18px;
  font-family: Arial, sans-serif;
}

/* 精灵图加载动画 */
.sprite-loader {
  width: 530px;
  /* 单帧宽度 */
  height: 150px;
  /* 单帧高度 */
  margin: 0 auto;
  background-image: url('@/assets/loading-sprite.png');
  /* 确保精灵图路径正确 */
  background-size: 1590px 1500px;
  /* 精灵图实际尺寸 */
  background-repeat: no-repeat;
  background-position: 0 0;
}

/* 保持原有样式不变 */
</style>