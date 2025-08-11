<template>
    <div class="map-background-container">

        <div class="map-background">
            <img src="/src/assets/map.png" alt="新艾利都地图" class="map-image" />
            <div v-for="(location, index) in locations" :key="index" class="map-marker" :style="{
                left: location.x + '%',
                top: location.y + '%'
            }" @click="openModal(location)">
                <div class="marker-icon">{{ location.icon }}</div>
            </div>
        </div>

        <!-- 弹窗组件 -->
        <Modal :visible="isModalVisible" :title="selectedLocation?.name || ''" @close="closeModal">
            <div class="location-details">
                <p>{{ selectedLocation?.description || '' }}</p>
            </div>

            <template #footer>
                <button class="modal-action-button" @click="closeModal">确定</button>
            </template>
        </Modal>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import Modal from './Modal.vue'

const isModalVisible = ref(false)
const selectedLocation = ref(null)

const locations = ref([
    {
        name: '零号空洞',
        x: 46,
        y: 5,
        icon: '🌀',
        description: '新艾利都最古老、规模最大的空洞，是所有其他大空洞的源头。它是新艾利都及周遭区域数个大空洞的源头，结构复杂、环境险恶令人谈之色变。由于曾引发几乎摧毁整座城市的灾难，它也由此成为恐怖的代名词。'
    },

    {
        name: '莱姆尼安空洞',
        x: 5,
        y: 70,
        icon: '🕳️',
        description: '六大空洞之一，规模惊人无法被剿灭。当空洞规模到达一定量级后，就会产生类似生物繁殖的增殖现象，诱发出新的「伴生空洞」，新艾利都频发的空洞灾害几乎都是由于这些大空洞的伴生增殖。'
    },


    {
        name: '旧普鲁托区',
        x: 49,
        y: 8,
        icon: '🏙️',
        description: '旧都陷落时被零号空洞吞噬的区域。距今十一年前，艾利都发生了一场规模空前的空洞灾害。零号空洞的活性在短时间内呈指数级增长，空洞规模急剧扩张，迅速吞没了艾利都当时最繁华的普鲁托区与密涅瓦区大面积土地。'
    },
    {
        name: '旧都',
        x: 55,
        y: 8,
        icon: '🏙️',
        description: '旧都的废墟，曾经是新艾利都的政治、经济和文化中心。如今，这里只剩下废墟和回忆。仍旧有人居住于旧都，这里黑帮与暴力团伙横行.'
    },
    {
        name: '新艾利都中心',
        x: 70,
        y: 25,
        icon: '🏢',
        description: '新艾利都重建后的市中心区域。在零号空洞的规模和活性都恢复正常后，幸存者在废墟之上擦干眼泪，重建了这座城市，并将其更名为新艾利都。这里代表着人类的坚韧和重生。'
    }
])

const openModal = (location) => {
    selectedLocation.value = location
    isModalVisible.value = true
}

const closeModal = () => {
    isModalVisible.value = false
    selectedLocation.value = null
}
</script>

<style scoped>
.map-background-container {
    position: relative;
    width: 100%;
    height: 100%;
    padding: 0px;
    margin: 0;
    box-sizing: border-box;
}

.map-background-container h3 {
    font-size: 28px;
    margin-bottom: 20px;
    text-align: center;
    color: #fff;
    position: relative;
    z-index: 11;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.8);
}

.map-background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 900px;
    overflow: hidden;
    z-index: 9;
}

.map-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
}

.map-marker {
    position: absolute;
    transform: translate(-50%, -50%);
    cursor: pointer;
    z-index: 10;
}

.marker-icon {
    font-size: 24px;
    background-color: rgba(0, 0, 0, 0.6);
    border-radius: 50%;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 2px solid white;
    transition: all 0.3s ease;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.5);
}

.map-marker:hover .marker-icon {
    transform: scale(1.2);
    background-color: rgba(255, 255, 255, 0.2);
}

.location-details p {
    margin: 0;
    font-size: 16px;
    line-height: 1.6;
    color: rgba(255, 255, 255, 0.9);
}

.modal-action-button {
    padding: 10px 20px;
    background: linear-gradient(to bottom, rgba(100, 100, 200, 0.7), rgba(80, 80, 180, 0.9));
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: all 0.3s ease;
    backdrop-filter: blur(5px);
    border: 1px solid rgba(255, 255, 255, 0.2);
}

.modal-action-button:hover {
    background: linear-gradient(to bottom, rgba(120, 120, 220, 0.8), rgba(100, 100, 200, 1));
    transform: translateY(-2px);
}

/* 响应式设计 */
@media (max-width: 768px) {
    .map-background-container h3 {
        font-size: 24px;
    }

    .marker-icon {
        width: 30px;
        height: 30px;
        font-size: 18px;
    }
}

@media (max-width: 480px) {
    .map-background-container {
        padding: 10px;
    }

    .map-background-container h3 {
        font-size: 20px;
    }
}
</style>