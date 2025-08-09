<template>
    <div class="app-header">
        <div class="header-content">
            <h1 class="header-title">{{ title }}</h1>
            <div class="header-actions">
                <slot name="actions">
                    <button v-if="showBack" @click="goBack" class="back-button">
                        {{ backText }}
                    </button>
                </slot>
            </div>
        </div>
    </div>
</template>

<script setup>
// 定义组件属性
const props = defineProps({
    title: {
        type: String,
        default: '应用标题'
    },
    showBack: {
        type: Boolean,
        default: true
    },
    backText: {
        type: String,
        default: '返回入口'
    }
})

// 定义事件
const emit = defineEmits(['back'])

// 返回事件处理
const goBack = () => {
    emit('back')
}
</script>

<style scoped>
.app-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 15px; /* 进一步减小padding */
    background: linear-gradient(to bottom, rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.4));
    backdrop-filter: blur(12px);
    border-bottom: 1px solid rgba(255, 255, 255, 0.15);
    position: relative;
    z-index: 10;
    width: 100%;
    box-sizing: border-box;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
}

.header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
}

.header-title {
    margin: 0;
    font-size: 24px; /* 减小字体大小 */
    color: white;
    text-shadow: 0 2px 8px rgba(0, 0, 0, 0.8);
    flex-shrink: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.header-actions {
    display: flex;
    align-items: center;
    flex-shrink: 0;
}

.back-button {
    padding: 8px 16px; /* 进一步减小按钮padding */
    background: linear-gradient(to bottom, rgba(255, 255, 255, 0.35), rgba(255, 255, 255, 0.2));
    color: white;
    border: 2px solid rgba(255, 255, 255, 0.9);
    border-radius: 8px;
    cursor: pointer;
    font-size: 14px; /* 减小字体大小 */
    font-weight: bold;
    transition: all 0.3s ease;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.8);
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.5);
    max-width: 150px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.back-button:hover {
    background: linear-gradient(to bottom, rgba(255, 255, 255, 0.5), rgba(255, 255, 255, 0.3));
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.6);
}

/* 媒体查询 - 针对小屏幕优化 */
@media (max-width: 768px) {
    .app-header {
        padding: 10px 10px;
    }
    
    .header-title {
        font-size: 20px;
    }
    
    .back-button {
        padding: 6px 12px;
        font-size: 12px;
        max-width: 120px;
    }
}

@media (max-width: 480px) {
    .header-title {
        font-size: 18px;
    }
    
    .back-button {
        padding: 5px 10px;
        font-size: 11px;
        max-width: 100px;
    }
}
</style>