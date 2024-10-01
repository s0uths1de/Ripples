<template>
  <div id="text" class="centered-text">
    <span class="textInner">{{ textInner }}</span>
    <span class="typed-cursor typed-cursor--blink" aria-hidden="true">&nbsp;_</span>
  </div>
</template>

<script setup>
import {ref, onMounted, onBeforeUnmount} from 'vue';

// 定义 props
const props = defineProps({
  text: {
    type: String,
    default: ''
  }
});

const textInner = ref('');
let intervalId = null; // 用于存储定时器 ID

const typewriterEffect = (text) => {
  let charIndex = 0;
  const delayPerChar = text.length > 20 ? 100 : 200;

  intervalId = setInterval(() => {
    if (charIndex < text.length) {
      textInner.value += text[charIndex];
      charIndex++;
    } else {
      clearInterval(intervalId);
    }
  }, delayPerChar);
};

onMounted(() => {
  setTimeout(() => {
    typewriterEffect(props.text);
  }, 200);
});

// 在组件卸载之前清除定时器
onBeforeUnmount(() => {
  clearInterval(intervalId);
});

</script>

<style scoped>
body {
  background-color: black;
  margin: 0;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.typed-cursor, .textInner {
  display: inline-block;
}

.typed-cursor--blink {
  animation: blink 0.7s infinite;
}

@keyframes blink {
  50% {
    opacity: 0;
  }
}

.centered-text {
  color: white;
  font-size: 24px;
  font-weight: bold;
  text-shadow: 1px 1px 2px black;
  white-space: nowrap;
  text-align: center;
}

.centered-text:hover {
  background: rgba(255, 255, 255, .5);
  backdrop-filter: blur(3px);
  border-radius: 15px;
  height: 40px;
  line-height: 40px;
}

#text{
  padding: 0 16px 0 26px;
}

</style>
