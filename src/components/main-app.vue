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
            <AppHeader title="zzzlibrary" :show-back="true" back-text="return" @back="goBack" />

            <div class="main-content">
                <div class="content">
                    <h2>欢迎来到主应用</h2>
                    <p>这里是你的主应用内容区域</p>

                    <!-- 在这里添加你的主应用内容 -->
                    <div class="sample-content">
                        <div class="card" v-for="item in items" :key="item.id">
                            <img :src="item.image" :alt="item.title" class="card-image" v-if="item.image">
                            <h3>{{ item.title }}</h3>
                            <p>{{ item.description }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import AppHeader from './AppHeader.vue'
// 导入图片资源
import image1 from '../assets/8月9日 (1)(1).png'
import image2 from '../assets/8月9日 (1)(2).png'
import image3 from '../assets/8月9日 (1)(3).png'
import image4 from '../assets/8月9日 (1)(4).png'
import image5 from '../assets/8月9日 (1)(6).png'
import image6 from '../assets/8月9日 (1)(7).png'
import image7 from '../assets/8月9日 (1).png'

const emit = defineEmits(['navigate'])

const goBack = () => {
    emit('navigate', 'welcome')
}

// 示例数据，包含图片
const items = [
    { id: 1, title: '功能一', description: '这是第一个功能描述', image: image1 },
    { id: 2, title: '功能二', description: '这是第二个功能描述', image: image2 },
    { id: 3, title: '功能三', description: '这是第三个功能描述', image: image3 },
    { id: 4, title: '功能四', description: '这是第四个功能描述', image: image4 },
    { id: 5, title: '功能五', description: '这是第五个功能描述', image: image5 },
    { id: 6, title: '功能六', description: '这是第六个功能描述', image: image6 },
    { id: 7, title: '功能七', description: '这是第七个功能描述', image: image7 }
]
</script>

<style scoped>
#main-app {
    height: 100vh;
    width: 100vw;
    font-family: Arial, sans-serif;
    color: white;
    position: relative;
    overflow: auto; /* 改为auto以允许滚动 */
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
    min-height: 100vh; /* 改为最小高度而不是固定高度 */
    width: 100vw;
    transform: translateZ(0);
    /* 添加轻微的背景遮罩以提高文字可读性 */
    background: rgba(0, 0, 0, 0.3);
}

.main-content {
    flex: 1;
    overflow-y: auto;
}

.content {
    padding: 40px;
    transform: translateZ(0);
}

.content h2 {
    font-size: 32px;
    margin-bottom: 10px;
    /* 增强文字的视差效果 */
    transform: translateZ(20px);
}

.content p {
    font-size: 18px;
    margin-bottom: 30px;
    transform: translateZ(10px);
}

.sample-content {
    display: flex;
    flex-direction: column;
    gap: 20px;
    align-items: center;
    padding-bottom: 40px; /* 添加底部内边距确保内容不被截断 */
}

.card {
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    padding: 20px;
    backdrop-filter: blur(10px);
    transition: transform 0.3s ease;
    /* 为卡片添加更强的视差效果 */
    transform: translateZ(30px);
    display: flex;
    flex-direction: column;
    width: 80%;
    max-width: 600px;
}

.card:hover {
    transform: translateZ(30px) translateY(-5px);
}

.card-image {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 5px;
    margin-bottom: 15px;
}

.card h3 {
    margin-top: 0;
    font-size: 20px;
}

.card p {
    font-size: 16px;
    margin-bottom: 0;
}
</style>