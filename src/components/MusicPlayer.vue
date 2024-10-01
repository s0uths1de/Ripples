<template>
  <div
      class="music-player"
      :style="{ left: position.x + 'px', top: position.y + 'px' }"
      @mousedown="startDrag"
  >
    <audio ref="audioElement" :src="musicUrl" @timeupdate="updateCurrentTime"></audio>
    <div class="controls">
      <button @click="togglePlay">{{ isPlaying ? '暂停' : '播放' }}</button>
      <input
          type="range"
          v-model="volume"
          @input="changeVolume"
          @mouseenter="startScroll"
          @mouseleave="stopScroll"
          @wheel="onScroll"
          min="0"
          max="1"
          step="0.01"
          class="volume-slider"
      />
      <span>{{ (volume * 100).toFixed(0) }}%</span>
    </div>
  </div>
</template>

<script>
import { ref, watch, onMounted } from 'vue';

export default {
  name: 'MusicPlayer',
  props: {
    musicUrl: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const audioElement = ref(new Audio());
    const isPlaying = ref(false);
    const volume = ref(0.3); // 默认音量
    const currentTime = ref(0);
    const duration = ref(0);


    // 拖动位置管理
    const position = ref({ x: 20, y: 20 }); // 初始化位置
    let isDragging = false; // 是否正在拖动
    let offset = { x: 0, y: 0 }; // 鼠标偏移量

    const togglePlay = () => {
      if (audioElement.value) {
        if (isPlaying.value) {
          audioElement.value.pause();
        } else {
          audioElement.value.play();
        }
        isPlaying.value = !isPlaying.value;
      }
    };

    const changeVolume = () => {
      if (audioElement.value) {
        audioElement.value.volume = volume.value;
      }
    };

    const updateCurrentTime = () => {
      if (audioElement.value) {
        currentTime.value = audioElement.value.currentTime;
        duration.value = audioElement.value.duration;
      }
    };

    watch(() => props.musicUrl, (newUrl) => {
      if (audioElement.value) {
        audioElement.value.src = newUrl;
        audioElement.value.load();
        isPlaying.value = false;
        currentTime.value = 0;
      }
    });


    onMounted(() => {
      if (audioElement.value) {
        audioElement.value.volume = volume.value;
      }
    });

    const startDrag = (event) => {
      isDragging = true;
      offset.x = event.clientX - position.value.x;
      offset.y = event.clientY - position.value.y;
      window.addEventListener('mousemove', onDrag);
      window.addEventListener('mouseup', stopDrag);
    };

    const startScroll = () => {
      window.addEventListener('wheel',onScroll)
    }

    const onScroll = (event) => {
      event.preventDefault();
      if (event.deltaY <0){
        volume.value = Math.min(1, volume.value + 0.01);
      }else if (event.deltaY>0){
        volume.value = Math.max(0, volume.value - 0.01);
      }
      if (audioElement.value) {
        audioElement.value.volume = volume.value;
      }
    }

    const stopScroll =()=>{
      window.removeEventListener('wheel',onScroll)

    }

    const onDrag = (event) => {
      if (isDragging) {
        let newX = event.clientX - offset.x;
        let newY = event.clientY - offset.y;

        // 限制组件不超出窗口边界
        newX = Math.max(0, Math.min(window.innerWidth - 160, newX)); // 200 是组件的宽度
        newY = Math.max(0, Math.min(window.innerHeight - 60, newY)); // 100 是组件的高度

        position.value = { x: newX, y: newY };
      }
    };

    const stopDrag = () => {
      isDragging = false;
      window.removeEventListener('mousemove', onDrag);
      window.removeEventListener('mouseup', stopDrag);
    };

    return {
      audioElement,
      isPlaying,
      volume,
      togglePlay,
      changeVolume,
      currentTime,
      duration,
      position,
      startDrag,
      stopScroll,
      startScroll,
      onScroll
    };
  },
};
</script>

<style scoped>
.music-player {
  position: fixed;
  width: 160px; /* 组件的固定宽度 */
  height: 20px; /* 组件的固定高度 */
  background-color: rgba(255, 255, 255, 0.9);
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: move;
}

.controls {
  display: flex;
  align-items: center;
  gap: 10px;
}

button {
  padding: 6px 8px;
  border: none;
  background-color: #42b983;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #36a570;
}

input[type="range"] {
  cursor: pointer;
  width: 60px; /* 控制滑块宽度 */
}

.volume-slider {
  appearance: none; /* 可自定义样式 */
  height: 5px;
  background: #ddd;
  border-radius: 5px;
}

.volume-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 15px; /* 滑块的大小 */
  height: 15px;
  border-radius: 50%;
  background: #42b983;
  cursor: pointer;
}
</style>
