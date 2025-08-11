<template>
    <div class="ai-content">
        <h3>四大人工智能(钥匙)</h3>
        <div class="ai-grid">
            <div class="ai-card" v-for="(ai, index) in aiList" :key="index">
                <div class="ai-icon">{{ ai.icon }}</div>
                <h4>{{ ai.name }}</h4>
                <p class="ai-role">{{ ai.role }}</p>
                <p class="ai-description">{{ ai.description }}</p>
            </div>
        </div>
    </div>
</template>

<script setup>
const aiList = [
    {
        icon: '🧚',
        name: 'Fairy',
        role: '数据收集专家',
        description: '擅长数据收集，还能控制各类电器。作为信息收集的核心AI，Fairy能够渗透到各种电子设备中，获取所需信息并控制智能家居设备。'
    },
    {
        icon: '👻',
        name: 'Ghost',
        role: '网络入侵专家',
        description: '擅长入侵各类系统和网络。Ghost具备强大的网络渗透能力，能够突破防火墙和安全系统，获取机密信息或控制系统功能。'
    },
    {
        icon: '👹',
        name: 'Youkai',
        role: '腐化专家',
        description: '擅长腐化和感染系统。Youkai能够将普通设备转化为被感染的节点，扩展其影响力并创建隐蔽的控制网络。'
    },
    {
        icon: '🧞',
        name: 'Jinni',
        role: '数据处理专家',
        description: '擅长打包上传和数据处理。Jinni能够高效地整理、压缩和传输大量数据，确保信息能够安全快速地传送到指定位置。'
    }
]
</script>

<style scoped>
.ai-content {
    padding: 20px;
}

.ai-content h3 {
    font-size: 28px;
    margin-bottom: 30px;
    text-align: center;
    color: #fff;
}

.ai-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 25px;
    margin-top: 20px;
}

.ai-card {
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 15px;
    padding: 25px 20px;
    backdrop-filter: blur(10px);
    transition: all 0.3s ease;
    text-align: center;
    border: 1px solid rgba(255, 255, 255, 0.2);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
}

.ai-card:hover {
    transform: translateY(-8px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.4);
    background-color: rgba(255, 255, 255, 0.15);
}

.ai-icon {
    font-size: 48px;
    margin-bottom: 15px;
}

.ai-card h4 {
    margin: 0 0 10px 0;
    font-size: 24px;
    color: #fff;
}

.ai-role {
    font-size: 16px;
    font-weight: bold;
    color: #a0a0ff;
    margin: 0 0 15px 0;
}

.ai-description {
    font-size: 15px;
    margin-bottom: 0;
    color: #e0e0e0;
    line-height: 1.5;
}

/* 响应式设计 */
@media (max-width: 768px) {
    .ai-content {
        padding: 15px;
    }

    .ai-content h3 {
        font-size: 24px;
        margin-bottom: 20px;
    }

    .ai-grid {
        gap: 20px;
    }

    .ai-card {
        padding: 20px 15px;
    }

    .ai-card h4 {
        font-size: 20px;
    }

    .ai-description {
        font-size: 14px;
    }
}

@media (max-width: 480px) {
    .ai-content h3 {
        font-size: 22px;
    }

    .ai-grid {
        grid-template-columns: 1fr;
    }

    .ai-icon {
        font-size: 40px;
    }
}
</style>