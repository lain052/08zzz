<template>
  <div id="welcome">
    
    <!-- 添加视频 -->
    <video muted loop playsinline class="background-video" ref="videoPlayer">
      <source src="@/assets/background-video.mp4" type="video/mp4">
    </video>
    
    <!-- 雪花屏效果层 -->
    <div v-show="showSnow" id="snow-screen"></div>
    
    <img id="musk" src="@/assets/musk.png" alt="Musk" />
    
    <!-- 电源按钮（控制雪花屏显示/隐藏） -->
    <img 
      id="power-button" 
      :class="{ rotated: isPowerButtonRotated }"
      src="@/assets/button.png" 
      alt="Power Button" 
      @click="togglePower"
    />
    
    <!-- 视频播放控制按钮 -->
    <img 
      id="play-button" 
      :class="{ rotated: isPlayButtonRotated, disabled: showSnow }"
      src="@/assets/button.png" 
      alt="Play Button" 
      @click="togglePlay"
    />
    
    <!-- 隐藏的音频元素 -->
    <audio ref="clickSound" preload="auto">
      <source src="@/assets/click.mp3" type="audio/mpeg">
    </audio>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isPowerOn: false,     // 电源状态（false=雪花屏，true=视频显示）
      isPlaying: false,     // 播放状态
      isPowerButtonRotated: false,
      isPlayButtonRotated: false,
      showSnow: true,       // 初始显示雪花屏
      snowInterval: null,
      currentFrame: 0,
      // 8×7精灵图总共有56帧
      totalFrames: 56
    }
  },
  
  mounted() {
    // 组件挂载后立即开始雪花动画
    this.startSnowAnimation();
    // 初始状态下不播放视频
    if (this.$refs.videoPlayer) {
      this.$refs.videoPlayer.pause();
    }
  },
  
  methods: {
    togglePower() {
      // 播放点击声音
      const sound = this.$refs.clickSound;
      sound.currentTime = 0;
      sound.play().catch(e => console.log("音频播放失败:", e));
      
      // 切换电源状态
      this.isPowerOn = !this.isPowerOn;
      
      const video = this.$refs.videoPlayer;
      if (this.isPowerOn) {
        // 电源打开：显示视频
        this.showSnow = false;
        this.stopSnowAnimation();
        
        // 如果播放按钮也是打开状态，则播放视频
        if (this.isPlaying) {
          video.play();
        } else {
          video.pause();
        }
      } else {
        // 电源关闭：显示雪花屏
        video.pause();
        this.showSnow = true;
        this.startSnowAnimation();
      }
      
      // 切换旋转状态
      this.isPowerButtonRotated = !this.isPowerButtonRotated;
    },
    
    togglePlay() {
      // 播放点击声音
      const sound = this.$refs.clickSound;
      sound.currentTime = 0;
      sound.play().catch(e => console.log("音频播放失败:", e));
      
      // 切换播放状态
      this.isPlaying = !this.isPlaying;
      
      const video = this.$refs.videoPlayer;
      if (this.isPlaying) {
        // 如果电源是打开的，则播放视频
        if (this.isPowerOn) {
          video.play();
        }
      } else {
        // 暂停视频
        video.pause();
      }
      
      // 切换旋转状态
      this.isPlayButtonRotated = !this.isPlayButtonRotated;
    },
    
    startSnowAnimation() {
      // 确保不会重复启动定时器
      this.stopSnowAnimation();
      
      this.snowInterval = setInterval(() => {
        this.currentFrame = (this.currentFrame + 1) % this.totalFrames;
        this.updateSnowBackground();
      }, 40);
    },
    
    stopSnowAnimation() {
      if (this.snowInterval) {
        clearInterval(this.snowInterval);
        this.snowInterval = null;
      }
    },
    
    updateSnowBackground() {
      const snowScreen = document.getElementById('snow-screen');
      if (snowScreen) {
        // 容器尺寸
        const containerWidth = 600;
        const containerHeight = 450;
        
        // 列数和行数
        const cols = 8;
        const rows = 7;
        
        // 计算每帧在放大后的背景中的位置
        const row = Math.floor(this.currentFrame / cols);
        const col = this.currentFrame % cols;
        
        // 计算背景定位（基于放大后的背景尺寸）
        const xPos = col * containerWidth;
        const yPos = row * containerHeight;
        
        snowScreen.style.backgroundPosition = `-${xPos}px -${yPos}px`;
      }
    }
  },
  
  beforeUnmount() {
    // 组件销毁前清理定时器
    this.stopSnowAnimation();
  }
}
</script>

<style scoped>
#welcome {
  background-image: url('@/assets/background.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  height: 100vh;
  width: 100vw;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.background-video {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 600px;
  height: 450px;
  transform: translate(-50%, -55%);
  z-index: 2; 
  object-fit: cover;
}

/* 雪花屏效果 */
#snow-screen {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 600px;
  height: 450px;
  transform: translate(-50%, -55%);
  z-index: 3;
  /* 使用精灵图作为背景 */
  background-image: url('@/assets/screen.jpg');
  /* 关键修改：让背景尺寸正好是容器尺寸的倍数 */
  background-size: 4800px 3150px; /* 8帧×600px, 7帧×450px */
  background-repeat: no-repeat;
  background-position: 0 0;
  /* 完全不透明 */
  opacity: 1;
  object-fit: cover;
  /* 添加黑色背景作为后备 */
  background-color: #000;
}

#musk {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 710px;
  height: 530px;
  transform: translate(-50%, -53.5%);
  z-index: 4;
  object-fit: cover;
}

#power-button {
  position: absolute;
  top: calc(50% - 222px);
  left: calc(50% + 390px);
  width: 72px;
  height: 72px;
  z-index: 5;
  cursor: pointer;
  object-fit: contain;
  background: transparent;
  filter: drop-shadow(0 0 2px rgba(0,0,0,0.3));
  transition: transform 0.3s ease;
  transform-origin: center center;
}

#power-button.rotated {
  transform: rotate(30deg);
}

#play-button {
  position: absolute;
  top: calc(50% - 130px); /* 在电源按钮下方 */
  left: calc(50% + 390px);
  width: 72px;
  height: 72px;
  z-index: 5;
  cursor: pointer;
  object-fit: contain;
  background: transparent;
  filter: drop-shadow(0 0 2px rgba(0,0,0,0.3));
  transition: transform 0.3s ease;
  transform-origin: center center;
}

#play-button.rotated {
  transform: rotate(30deg);
}

#play-button.disabled {
  cursor: not-allowed;
  filter: drop-shadow(0 0 2px rgba(0,0,0,0.3)) grayscale(100%);
}

</style>