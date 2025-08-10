<script setup>
import welcome from './components/welcome.vue'
import mainApp from './components/main-app.vue'
import { ref, onMounted, onBeforeUnmount } from 'vue'

// 加载状态
const isAppLoaded = ref(false)
const currentPage = ref('welcome')
const spriteInterval = ref(null)
const spriteFrame = ref(0)
const totalSpriteFrames = 30 // 3*10精灵图总帧数
const showPageTransition = ref(false) // 页面跳转时的加载动画

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

// 处理页面跳转
const handleNavigation = (page) => {
  if (page === 'main') {
    // 显示页面跳转动画
    showPageTransition.value = true
    startSpriteAnimation()

    // 模拟加载时间（可以根据实际需要调整）
    setTimeout(() => {
      currentPage.value = page
      showPageTransition.value = false
      stopSpriteAnimation()
    }, 2000)
  } else {
    currentPage.value = page
  }
}

// 实际加载应用资源
const loadAppResources = async () => {
  try {
    // 并行加载多个资源（不包含固定延迟）
    await Promise.all([
      // 加载配置文件
      fetch('/config/app-config.json').then(res => res.json()).catch(() => ({})),

      // 预加载关键图片资源
      preloadImage('/assets/background.jpg').catch(() => { }),
      preloadImage('/assets/background-video.mp4').catch(() => { }),
      preloadImage('/assets/musk.png').catch(() => { }),
      preloadImage('/assets/button.png').catch(() => { }),
      preloadImage('/assets/screen.jpg').catch(() => { }),

      // 预加载音频资源
      preloadAudio('/assets/click.mp3').catch(() => { })
    ])

    // 资源加载完成后，再额外等待2.5秒
    await new Promise(resolve => setTimeout(resolve, 2500))

    // 所有资源加载完成并且额外等待2.5秒后
    isAppLoaded.value = true
    stopSpriteAnimation()
  } catch (error) {
    console.error('资源加载失败:', error)
    // 即使出错也显示主界面
    isAppLoaded.value = true
    stopSpriteAnimation()
  }
}

// 预加载图片
const preloadImage = (src) => {
  return new Promise((resolve, reject) => {
    const img = new Image()
    img.onload = resolve
    img.onerror = reject
    img.src = src
  })
}

// 预加载音频
const preloadAudio = (src) => {
  return new Promise((resolve, reject) => {
    const audio = new Audio()
    audio.addEventListener('canplaythrough', resolve)
    audio.addEventListener('error', reject)
    audio.src = src
    audio.load()
  })
}

onMounted(() => {
  startSpriteAnimation()
  loadAppResources()
})

onBeforeUnmount(() => {
  stopSpriteAnimation()
})
</script>

<template>
  <!-- 页面跳转加载动画 -->
  <div v-if="showPageTransition" class="app-loading">
    <div class="loading-content">
      <div class="sprite-loader"></div>
    </div>
  </div>
  <!-- 全局加载动画 -->
  <div v-if="!isAppLoaded" class="app-loading">
    <div class="loading-content">
      <div class="sprite-loader"></div>
    </div>
  </div>

  <!-- 应用主内容 -->
  <div v-show="isAppLoaded && !showPageTransition" class="app-content">
    <welcome v-if="currentPage === 'welcome'" @navigate="handleNavigation" />
    <main-app v-else-if="currentPage === 'main'" @navigate="handleNavigation" />
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
}

.loading-content p {
  margin-top: 30px;
  font-size: 18px;
  font-family: Arial, sans-serif;
  color: #ffffff;
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

/* 全屏应用内容样式 */
.app-content {
  height: 100vh;
  width: 100vw;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

/* 移除默认边距 */
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  overflow: hidden;
}

/* 确保组件全屏 */
#welcome,
#main-app {
  height: 100vh !important;
  width: 100vw !important;
  margin: 0 !important;
  padding: 0 !important;
}
</style>