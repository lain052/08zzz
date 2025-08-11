<template>
    <div class="items-content">
        <h3>关键物品</h3>
        <div class="items-grid">
            <div class="item-card" v-for="(item, index) in items" :key="index">
                <div class="item-icon">
                    <span>{{ item.icon }}</span>
                </div>
                <h4>{{ item.name }}</h4>
                <p>{{ item.description }}</p>
            </div>
        </div>
    </div>
</template>

<script setup>
const items = [
    {
        icon: '🔍',
        name: '以太探测器',
        description: '用于探测空洞中以太浓度的专业设备，是调查员执行任务时的必备工具。'
    },
    {
        icon: '🛡️',
        name: '防护服',
        description: '特制的防护装备，能够抵御一定程度的以太侵蚀，保护调查员安全。'
    },
    {
        icon: '💊',
        name: '抗侵蚀药剂',
        description: '用于缓解以太侵蚀带来的负面影响，是调查员在空洞中生存的重要保障。'
    },
    {
        icon: '⚙️',
        name: '式舆塔',
        description: '新艾利都的重要设施，既能作为防御装置，也具备引爆功能以控制空洞扩张。新艾利都下方有传输以太资源的小型式舆塔，以及管道传输网络'
    },
    {
        icon: '🤖',
        name: '邦布',
        description: '由挽昼发明的人工智能辅助设备，具有多种功能，是调查员的得力助手。'
    }

]
</script>

<style scoped>
.items-content {
    padding: 20px;
}

.items-content h3 {
    font-size: 28px;
    margin-bottom: 20px;
    text-align: center;
    color: #fff;
}

.items-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 25px;
    margin-top: 20px;
}

.item-card {
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 15px;
    padding: 25px 20px;
    backdrop-filter: blur(10px);
    transition: all 0.3s ease;
    text-align: center;
    border: 1px solid rgba(255, 255, 255, 0.2);
}

.item-card:hover {
    transform: translateY(-8px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
    background-color: rgba(255, 255, 255, 0.15);
}

.item-icon {
    font-size: 40px;
    margin-bottom: 15px;
}

.item-card h4 {
    margin: 0 0 15px 0;
    font-size: 22px;
    color: #fff;
}

.item-card p {
    font-size: 16px;
    margin-bottom: 0;
    color: #e0e0e0;
    line-height: 1.5;
}
</style>