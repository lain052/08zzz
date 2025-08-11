<template>
    <div id="main-app">
        <!-- 视频背景 -->
        <div class="video-background">
            <video autoplay muted loop class="background-video" playsinline>
                <source src="/src/assets/background-video2.mp4" type="video/mp4">
                您的浏览器不支持视频播放。
            </video>
        </div>

        <!-- 内容层 - 添加视差效果 -->
        <div class="parallax-content">
            <!-- 使用独立的 Header 组件 -->
            <AppHeader title="zzzlibrary" :show-back="true" back-text="return" @back="goBack">
                <template #actions>
                    <div class="header-tabs">
                        <button class="tab-button" :class="{ active: activeTab === 'overview' }"
                            @click="activeTab = 'overview'">
                            概览
                        </button>
                        <button class="tab-button" :class="{ active: activeTab === 'timeline' }"
                            @click="activeTab = 'timeline'">
                            时间线
                        </button>
                        <button class="tab-button" :class="{ active: activeTab === 'factions' }"
                            @click="activeTab = 'factions'">
                            势力团体
                        </button>
                        <button class="tab-button" :class="{ active: activeTab === 'disasters' }"
                            @click="activeTab = 'disasters'">
                            空洞灾厄
                        </button>
                        <button class="tab-button" :class="{ active: activeTab === 'items' }"
                            @click="activeTab = 'items'">
                            关键物品
                        </button>
                    </div>
                </template>
            </AppHeader>

            <div class="main-content" ref="mainContent">
                <div class="content">
                    <h2>zzz</h2>


                    <!-- 概览内容 -->
                    <div v-show="activeTab === 'overview'" class="tab-content">
                        <div class="spacer-before">
                            <div class="scroll-instruction">虚狩</div>
                        </div>
                        <div class="horizontal-scroll-wrapper" ref="scrollWrapper">
                            <div class="horizontal-container" ref="horizontalContainer">
                                <div class="cards-container" ref="cardsContainer">
                                    <div class="card" v-for="(item) in items" :key="item.id">
                                        <img :src="item.image" :alt="item.title" class="card-image" v-if="item.image">
                                        <h3>{{ item.title }}</h3>
                                        <p class="card-description">{{ item.description }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="spacer-after">
                            <div class="scroll-instruction">虚狩</div>
                        </div>
                    </div>

                    <!-- 时间线内容 -->
                    <div v-show="activeTab === 'timeline'" class="tab-content">
                        <Timeline />
                    </div>

                    <!-- 势力团体内容 -->
                    <div v-show="activeTab === 'factions'" class="tab-content">
                        <Factions />
                    </div>

                    <!-- 空洞灾厄内容 -->
                    <div v-show="activeTab === 'disasters'" class="tab-content">
                        <Disasters />
                    </div>

                    <!-- 关键物品内容 -->
                    <div v-show="activeTab === 'items'" class="tab-content">
                        <Items />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, nextTick, watch } from 'vue'
import AppHeader from './AppHeader.vue'
import Timeline from './Timeline.vue'
import Factions from './Factions.vue'
import Disasters from './Disasters.vue'
import Items from './Items.vue' // 导入 Items 组件
// 导入图片资源
import image1 from '../assets/8月9日 (1).png'
import image2 from '../assets/8月9日 (1)(2).png'
import image3 from '../assets/8月9日 (1)(3).png'
import image4 from '../assets/8月9日 (1)(6).png'
import image5 from '../assets/8月9日 (1)(4).png'
import image6 from '../assets/8月9日 (1)(7).png'
import image7 from '../assets/8月9日 (1)(1).png'

const emit = defineEmits(['navigate'])

const goBack = () => {
    emit('navigate', 'welcome')
}

// 默认激活的标签页
const activeTab = ref('overview')

// 水平滚动相关元素引用
const mainContent = ref(null)
const scrollWrapper = ref(null)
const horizontalContainer = ref(null)
const cardsContainer = ref(null)

// 示例数据，包含图片 - 修复了重复ID和图片引用错误
const items = [
    { id: 1, title: '星见家三代家主', description: '号称“剑圣”，一己之力斩杀称颂会司教', image: image1 },
    { id: 2, title: '乔伊斯', description: '调查公会ACE。只身穿越“神的迷宫”，建立空洞探索路径标准方案。是调查员和绳匠的先祖', image: image2 },
    { id: 3, title: '维克', description: '节杖军上校。与丹中校和法金汉佣兵团团长联手将“黑墙”斥退37公里', image: image3 },
    { id: 4, title: '丹', description: '节杖军中校。与维克上校和法金汉佣兵团团长联手将“黑墙”斥退37公里', image: image4 },
    { id: 5, title: '法金汉佣兵团团长', description: '与节杖军的维克上校和丹中校联手将“黑墙”斥退37公里', image: image5 },
    { id: 6, title: '亚契', description: '赫利俄斯机关创始人，成功揭露大多数空洞特性的学者。为追寻灾难真相而进入零号空洞内，尚未从零号空洞中归还', image: image6 },
    { id: 7, title: '挽昼', description: '玛瑟尔集团时任首席执行官。成功消解天上的“巢主”空洞。邦布的发明人', image: image7 }
]

let cleanupFunction = null
let lastTranslateX = 0

// 初始化水平滚动效果
const initHorizontalScroll = async () => {
    // 等待DOM更新完成
    await nextTick()

    if (!scrollWrapper.value || !horizontalContainer.value || !cardsContainer.value || !mainContent.value) {
        console.log('缺少必要的DOM元素')
        return
    }

    // 参考示例的计算方式：距离 = 卡片容器总宽度 - 窗口宽度
    const cardsWidth = cardsContainer.value.offsetWidth || 0
    const windowWidth = window.innerWidth || 0
    const distance = Math.max(0, cardsWidth - windowWidth)

    // 设置包装器高度等于滚动距离，与示例保持一致
    scrollWrapper.value.style.height = `${distance}px`

    console.log('初始化水平滚动:', {
        distance,
        cardsWidth,
        windowWidth,
        windowHeight: window.innerHeight
    })

    // 创建滚动处理函数
    const handleScroll = () => {
        if (!scrollWrapper.value || !mainContent.value) return

        const wrapperRect = scrollWrapper.value.getBoundingClientRect()
        const windowHeight = window.innerHeight
        const wrapperHeight = scrollWrapper.value.offsetHeight

        // 精确判断包装器是否完全固定在视窗中
        if (wrapperRect.top <= 0 && wrapperRect.bottom >= windowHeight) {
            // 当包装器完全占据视窗时，开始水平滚动
            // 使用更精确的进度计算方式
            const progress = Math.abs(wrapperRect.top) / (wrapperHeight - windowHeight)
            const clampedProgress = Math.max(0, Math.min(1, progress))

            const translateX = -clampedProgress * distance

            // 避免不必要的重复渲染
            if (Math.abs(translateX - lastTranslateX) > 0.1) {
                cardsContainer.value.style.transform = `translateX(${translateX}px)`
                lastTranslateX = translateX
            }

            console.log('滚动进度:', { progress: clampedProgress, translateX })
        } else if (wrapperRect.top > 0) {
            // 包装器还在下方，未到达视窗顶部
            if (lastTranslateX !== 0) {
                cardsContainer.value.style.transform = `translateX(0px)`
                lastTranslateX = 0
            }
        } else if (wrapperRect.bottom < windowHeight) {
            // 包装器已经离开视窗，完全滚动完成
            if (lastTranslateX !== -distance) {
                cardsContainer.value.style.transform = `translateX(-${distance}px)`
                lastTranslateX = -distance
            }
        }
    }

    // 添加滚动事件监听器
    mainContent.value.addEventListener('scroll', handleScroll, { passive: true })

    // 初始化位置
    handleScroll()

    // 返回清理函数
    const cleanup = () => {
        if (mainContent.value) {
            mainContent.value.removeEventListener('scroll', handleScroll)
        }
    }

    return cleanup
}

// 清理之前的监听器
const cleanupPrevious = () => {
    if (cleanupFunction) {
        cleanupFunction()
        cleanupFunction = null
    }
}

// 重新初始化水平滚动
const reinitHorizontalScroll = async () => {
    if (activeTab.value === 'overview') {
        // 延迟执行以确保DOM更新完成
        setTimeout(async () => {
            cleanupPrevious()
            cleanupFunction = await initHorizontalScroll()
        }, 100)
    } else {
        cleanupPrevious()
    }
}

// 添加滚动事件监听器
onMounted(async () => {
    console.log('组件已挂载')

    // 等待DOM渲染完成
    setTimeout(async () => {
        if (activeTab.value === 'overview') {
            console.log('初始化概览页面水平滚动')
            cleanupPrevious()
            cleanupFunction = await initHorizontalScroll()
        }
    }, 300)
})

// 监听标签页切换
watch(activeTab, (newVal) => {
    console.log('标签页切换到:', newVal)
    reinitHorizontalScroll()
})

// 组件销毁前清理
onBeforeUnmount(() => {
    console.log('组件即将销毁，清理资源')
    cleanupPrevious()
})
</script>

<style scoped>
#main-app {
    height: 100vh;
    width: 100%;
    font-family: Arial, sans-serif;
    color: white;
    position: relative;
    overflow: hidden;
}

/* 视频背景样式 */
.video-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -2;
    overflow: hidden;
}

.background-video {
    min-width: 100%;
    min-height: 100%;
    width: auto;
    height: auto;
    object-fit: cover;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

/* 内容层 - 用于视差效果 */
.parallax-content {
    position: relative;
    min-height: 100vh;
    width: 100%;
    transform: translateZ(0);
    background: rgba(0, 0, 0, 0.3);
}

.main-content {
    flex: 1;
    overflow-y: auto;
    height: 100vh;
    padding: 0;
    margin: 0;
    will-change: scroll-position;
}

.content {
    padding: 20px;
    transform: translateZ(0);
    box-sizing: border-box;
}

.content h2 {
    font-size: 32px;
    margin-bottom: 10px;
    transform: translateZ(20px);
    text-align: center;
}

.content p {
    font-size: 18px;
    margin-bottom: 30px;
    transform: translateZ(10px);
    text-align: center;
}

/* 空白间隔 */
.spacer-before,
.spacer-after {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    color: rgba(255, 255, 255, 0.7);
    background: linear-gradient(to bottom, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.3));
}

.spacer-after {
    height: 100vh;
}

.scroll-instruction {
    background: rgba(255, 255, 255, 0.1);
    padding: 20px 40px;
    border-radius: 10px;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
}

/* 水平滚动包装器 */
.horizontal-scroll-wrapper {
    position: relative;
    width: 100%;
}

.horizontal-container {
    position: sticky;
    top: 0;
    width: 100%;
    height: 100vh;
    display: flex;
    align-items: center;
    overflow: hidden;
    background: rgba(0, 0, 0, 0.2);
    backdrop-filter: blur(5px);
    will-change: contents;
}

.cards-container {
    display: flex;
    gap: 25px;
    padding: 0 40px;
    will-change: transform;
    align-items: center;
    height: 70vh;
}

/* 标签导航样式 */
.header-tabs {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}

.tab-button {
    background-color: rgba(255, 255, 255, 0.1);
    border: none;
    color: white;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s ease;
    backdrop-filter: blur(10px);
    font-size: 14px;
}

.tab-button:hover {
    background-color: rgba(255, 255, 255, 0.2);
}

.tab-button.active {
    background-color: rgba(255, 255, 255, 0.3);
    font-weight: bold;
}

.tab-content {
    min-height: 100vh;
}

/* 卡片样式 */
.card {
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 15px;
    padding: 22px;
    backdrop-filter: blur(15px);
    display: flex;
    flex-direction: column;
    min-width: 400px;
    width: 400px;
    /* 固定宽度 */
    flex-shrink: 0;
    height: 60vh;
    border: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
    transition: all 0.3s ease;
    will-change: transform;
}

.card:hover {
    transform: translateY(-10px);
    background-color: rgba(255, 255, 255, 0.15);
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
}

.card-image {
    width: 100%;
    height: 160px;
    object-fit: cover;
    border-radius: 10px;
    margin-bottom: 18px;
    flex-shrink: 0;
}

.card h3 {
    margin-top: 0;
    font-size: 20px;
    color: white;
    margin-bottom: 14px;
    flex-shrink: 0;
    text-align: center;
}

.card-description {
    font-size: 15px;
    margin-bottom: 0;
    color: rgba(255, 255, 255, 0.9);
    line-height: 1.5;
    overflow-y: auto;
    flex-grow: 1;
    text-align: justify;
}

/* 响应式设计 */
@media (max-width: 768px) {
    .header-tabs {
        justify-content: center;
    }

    .tab-button {
        padding: 6px 12px;
        font-size: 12px;
    }

    .card {
        min-width: 75vw;
        width: 75vw;
        /* 固定宽度 */
        height: 60vh;
        padding: 18px;
    }

    .cards-container {
        gap: 20px;
        padding: 0 20px;
    }

    .spacer-before,
    .spacer-after {
        font-size: 18px;
        height: 80vh;
    }

    .content h2 {
        font-size: 24px;
    }

    .content p {
        font-size: 16px;
    }

    .card h3 {
        font-size: 18px;
    }

    .card-description {
        font-size: 14px;
    }

    .card-image {
        height: 140px;
    }
}

@media (max-width: 480px) {
    .card {
        min-width: 80vw;
        width: 80vw;
        /* 固定宽度 */
        height: 55vh;
        padding: 15px;
    }

    .cards-container {
        gap: 15px;
        padding: 0 15px;
    }

    .spacer-before,
    .spacer-after {
        height: 60vh;
    }

    .card h3 {
        font-size: 16px;
    }

    .card-description {
        font-size: 13px;
    }

    .card-image {
        height: 120px;
    }
}
</style>