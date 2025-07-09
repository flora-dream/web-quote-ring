<template>
  <div class="quote-app" :style="{ backgroundImage: `url(${currentBackground})` }">
    <!-- 背景音乐控制按钮 -->
    <div class="bgm-control">
      <button @click="toggleBgm" class="bgm-btn" :class="{ active: isBgmPlaying }">
        <svg v-if="isBgmPlaying" viewBox="0 0 24 24" width="24" height="24">
          <path fill="white" d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/>
        </svg>
        <svg v-else viewBox="0 0 24 24" width="24" height="24">
          <path fill="white" d="M16.5 12c0-1.77-1.02-3.29-2.5-4.03v2.21l2.45 2.45c.03-.2.05-.41.05-.63zm2.5 0c0 .94-.2 1.82-.54 2.64l1.51 1.51C20.63 14.91 21 13.5 21 12c0-4.28-2.99-7.86-7-8.77v2.06c2.89.86 5 3.54 5 6.71zM4.27 3L3 4.27 7.73 9H3v6h4l5 5v-6.73l4.25 4.25c-.67.52-1.42.93-2.25 1.18v2.06c1.38-.31 2.63-.95 3.69-1.81L19.73 21 21 19.73l-9-9L4.27 3zM12 4L9.91 6.09 12 8.18V4z"/>
        </svg>
      </button>
    </div>

    <!-- 语录内容 -->
    <div class="quote-content">
      <blockquote class="quote-text">
        "{{ currentQuote.text }}"
      </blockquote>
      <cite class="quote-source">{{ currentQuote.source }}</cite>
    </div>

    <!-- 播放控制按钮 -->
    <div class="controls">
      <button @click="previousQuote" class="control-btn">
        <svg viewBox="0 0 24 24" width="32" height="32">
          <path fill="white" d="M6 6h2v12H6zm3.5 6l8.5 6V6z"/>
        </svg>
      </button>
      
      <button @click="togglePlay" class="control-btn play-btn">
        <svg v-if="isPlaying" viewBox="0 0 24 24" width="40" height="40">
          <path fill="white" d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/>
        </svg>
        <svg v-else viewBox="0 0 24 24" width="40" height="40">
          <path fill="white" d="M8 5v14l11-7z"/>
        </svg>
      </button>
      
      <button @click="nextQuote" class="control-btn">
        <svg viewBox="0 0 24 24" width="32" height="32">
          <path fill="white" d="M6 18l8.5-6L6 6v12zM16 6v12h2V6h-2z"/>
        </svg>
      </button>
    </div>

    <!-- 音频元素 -->
    <audio ref="quoteAudio" @ended="onAudioEnded"></audio>
    <audio ref="bgmAudio" loop @ended="onBgmEnded"></audio>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      quotes: [
        { 
          text: "家长越是想赢过孩子，越是会输掉孩子。",
          source: "爱的思辨",
          audio: "/audio/1.mp3"
        },
        { 
          text: "受委屈的孩子充满敌意，被感动的孩子充满智慧。",
          source: "爱的思辨",
          audio: "/audio/2.mp3"
        },
        { 
          text: "对孩子来说，规则虽然是一种约束，但更是一种保护。清晰的规则会让孩子体会到踏实和自在。",
          source: "爱的思辨",
          audio: "/audio/3.mp3"
        },
        { 
          text: "关于陪伴孩子，家长用10%的心，陪伴10小时，不如用100%的心，陪伴1小时。这就是为什么有的家长既能带好孩子，又能经营好自己。",
          source: "爱的思辨",
          audio: "/audio/4.mp3"
        },
        { 
          text: "父母不好好过日子，是对孩子最不好的影响。",
          source: "爱的思辨",
          audio: "/audio/5.mp3"
        }
      ],
      backgrounds: [
        "/image/1.jpg",
        "/image/2.jpg", 
        "/image/3.jpg",
        "/image/4.jpg",
        "/image/5.jpg"
      ],
      bgmList: [
        "/bgm/你的微笑 - 轻音乐网.mp3",
        "/bgm/清晨 - 轻音乐网.mp3"
      ],
      currentQuoteIndex: 0,
      currentBackground: "",
      currentBgm: "",
      isPlaying: false,
      isBgmPlaying: false
    }
  },
  computed: {
    currentQuote() {
      return this.quotes[this.currentQuoteIndex]
    }
  },
  mounted() {
    this.randomizeContent()
    this.setupAudio()
  },
  methods: {
    randomizeContent() {
      // 随机选择语录
      this.currentQuoteIndex = Math.floor(Math.random() * this.quotes.length)
      
      // 随机选择背景图
      const randomBgIndex = Math.floor(Math.random() * this.backgrounds.length)
      this.currentBackground = this.backgrounds[randomBgIndex]
      
      // 随机选择背景音乐
      const randomBgmIndex = Math.floor(Math.random() * this.bgmList.length)
      this.currentBgm = this.bgmList[randomBgmIndex]
      this.$refs.bgmAudio.src = this.currentBgm
    },
    
    setupAudio() {
      this.$refs.quoteAudio.src = this.currentQuote.audio
    },
    
    togglePlay() {
      if (this.isPlaying) {
        this.$refs.quoteAudio.pause()
        this.isPlaying = false
      } else {
        this.$refs.quoteAudio.play()
        this.isPlaying = true
      }
    },
    
    toggleBgm() {
      if (this.isBgmPlaying) {
        this.$refs.bgmAudio.pause()
        this.isBgmPlaying = false
      } else {
        this.$refs.bgmAudio.play()
        this.isBgmPlaying = true
      }
    },
    
    previousQuote() {
      this.isPlaying = false
      this.$refs.quoteAudio.pause()
      this.currentQuoteIndex = this.currentQuoteIndex > 0 ? this.currentQuoteIndex - 1 : this.quotes.length - 1
      this.setupAudio()
    },
    
    nextQuote() {
      this.isPlaying = false
      this.$refs.quoteAudio.pause()
      this.currentQuoteIndex = this.currentQuoteIndex < this.quotes.length - 1 ? this.currentQuoteIndex + 1 : 0
      this.setupAudio()
    },
    
    onAudioEnded() {
      this.isPlaying = false
    },
    
    onBgmEnded() {
      this.isBgmPlaying = false
    }
  }
}
</script>

<style scoped>
.quote-app {
  height: 100vh;
  width: 100vw;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  color: white;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
}

.quote-app::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.3);
  z-index: 1;
}

.bgm-control {
  position: absolute;
  top: 20px;
  right: 20px;
  z-index: 10;
}

.bgm-btn {
  background: rgba(255, 255, 255, 0.2);
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.bgm-btn:hover {
  background: rgba(255, 255, 255, 0.3);
  border-color: rgba(255, 255, 255, 0.5);
  transform: scale(1.1);
}

.bgm-btn.active {
  background: rgba(255, 255, 255, 0.4);
  border-color: rgba(255, 255, 255, 0.6);
}

.quote-content {
  z-index: 5;
  text-align: center;
  max-width: 800px;
  padding: 0 40px;
  margin-bottom: 80px;
}

.quote-text {
  font-size: 2.5rem;
  font-weight: 300;
  line-height: 1.4;
  margin-bottom: 30px;
  font-style: italic;
}

.quote-source {
  font-size: 1.5rem;
  opacity: 0.9;
  font-weight: 400;
}

.controls {
  z-index: 5;
  display: flex;
  align-items: center;
  gap: 20px;
}

.control-btn {
  background: rgba(255, 255, 255, 0.2);
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  width: 70px;
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.play-btn {
  width: 80px;
  height: 80px;
}

.control-btn:hover {
  background: rgba(255, 255, 255, 0.3);
  border-color: rgba(255, 255, 255, 0.5);
  transform: scale(1.1);
}

.control-btn:active {
  transform: scale(0.95);
}

/* 响应式设计 */
@media (max-width: 768px) {
  .quote-content {
    padding: 0 20px;
    margin-bottom: 60px;
  }
  
  .quote-text {
    font-size: 1.8rem;
  }
  
  .quote-source {
    font-size: 1.2rem;
  }
  
  .control-btn {
    width: 60px;
    height: 60px;
  }
  
  .play-btn {
    width: 70px;
    height: 70px;
  }
  
  .bgm-btn {
    width: 50px;
    height: 50px;
  }
}

@media (max-width: 480px) {
  .quote-text {
    font-size: 1.5rem;
  }
  
  .quote-source {
    font-size: 1rem;
  }
  
  .controls {
    gap: 15px;
  }
  
  .control-btn {
    width: 50px;
    height: 50px;
  }
  
  .play-btn {
    width: 60px;
    height: 60px;
  }
}
</style> 