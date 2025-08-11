<template>
    <div v-if="visible" class="modal-overlay" @click="closeModal">
        <div class="modal-container" @click.stop>
            <div class="modal-header">
                <h3>{{ title }}</h3>
                <button class="modal-close" @click="closeModal">&times;</button>
            </div>
            <div class="modal-content">
                <slot>
                    <p>{{ content }}</p>
                </slot>
            </div>
            <div class="modal-footer" v-if="$slots.footer">
                <slot name="footer"></slot>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    visible: {
        type: Boolean,
        default: false
    },
    title: {
        type: String,
        default: '弹窗标题'
    },
    content: {
        type: String,
        default: ''
    }
})

const emit = defineEmits(['close'])

const closeModal = () => {
    emit('close')
}

// 支持按 ESC 键关闭弹窗
import { onMounted, onUnmounted } from 'vue'

const handleKeydown = (event) => {
    if (event.key === 'Escape') {
        closeModal()
    }
}

onMounted(() => {
    document.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
    document.removeEventListener('keydown', handleKeydown)
})
</script>

<style scoped>
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    backdrop-filter: blur(5px);
}

.modal-container {
    background: linear-gradient(to bottom, rgba(30, 30, 40, 0.9), rgba(20, 20, 30, 0.95));
    border-radius: 15px;
    width: 90%;
    max-width: 600px;
    max-height: 80vh;
    overflow: hidden;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
    border: 1px solid rgba(255, 255, 255, 0.2);
    display: flex;
    flex-direction: column;
    animation: modalAppear 0.3s ease-out;
}

@keyframes modalAppear {
    from {
        opacity: 0;
        transform: translateY(-50px) scale(0.9);
    }

    to {
        opacity: 1;
        transform: translateY(0) scale(1);
    }
}

.modal-header {
    padding: 20px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.modal-header h3 {
    margin: 0;
    font-size: 24px;
    color: white;
}

.modal-close {
    background: none;
    border: none;
    color: rgba(255, 255, 255, 0.7);
    font-size: 28px;
    cursor: pointer;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: all 0.3s ease;
}

.modal-close:hover {
    background-color: rgba(255, 255, 255, 0.1);
    color: white;
}

.modal-content {
    padding: 25px;
    overflow-y: auto;
    flex-grow: 1;
}

.modal-content p {
    margin: 0;
    font-size: 16px;
    line-height: 1.6;
    color: rgba(255, 255, 255, 0.9);
}

.modal-footer {
    padding: 15px 25px;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    justify-content: flex-end;
    gap: 10px;
}

/* 响应式设计 */
@media (max-width: 768px) {
    .modal-container {
        width: 95%;
        max-height: 90vh;
    }

    .modal-header h3 {
        font-size: 20px;
    }

    .modal-content {
        padding: 20px;
    }

    .modal-content p {
        font-size: 14px;
    }
}

@media (max-width: 480px) {
    .modal-header {
        padding: 15px;
    }

    .modal-content {
        padding: 15px;
    }

    .modal-footer {
        padding: 10px 15px;
    }
}
</style>