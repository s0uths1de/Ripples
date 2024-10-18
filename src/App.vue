<template>
  <div style="position: relative; max-width: 100%;
  height: 100vh; overflow: hidden">
    <Typewriter class="typewrite" @click="clickCopy" v-if="initialText" :text="initialText"
                style="cursor: pointer; display:block;position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); z-index: 100"/>
    <div id="mouse" class="circle-out"></div>
    <img style="width: 100%; height: 100%;" :src="imgUrl" alt="Scenic Image"/>
    <div class="introduce">
      <Typewriter v-if="initialText" :text="ripples" @mouseenter="isDisplayInformation(false)"
                  style="color:white;font-size: 18px"/>
      <div id="information"
           class="information"
           @mouseleave="isDisplayInformation(true)">
        <div class="information-button" style="float: left">公告</div>
        <div class="information-button" style="float: right">赞助</div>
      </div>
    </div>
    <div class="footer" style="position: absolute; top: 95%; left: 50%; transform: translate(-50%, -50%);">
      <a href="https://github.com/s0uths1de">
        <Typewriter style="font-size: 12px;" text="S1de个人仓库 " :is-hover="false"/>
      </a>
      <br>
      <a href="https://beian.miit.gov.cn/">
        <Typewriter style="font-size: 12px;" text="赣ICP备2024033384号 " :is-hover="false"/>
      </a>
    </div>
  </div>

  <MusicPlayer :musicUrl="musicUrl"/>
</template>

<script setup>
import {ref, onMounted} from 'vue';
import Typewriter from '@/components/Typewriter.vue';
import MusicPlayer from "@/components/MusicPlayer.vue";
import axios from 'axios';

const musicUrlArr = ref([{
  name: '万葉の恋 - Jusqu\'à Grand-Père',
  url: 'http://music.163.com/song/media/outer/url?id=756141.mp3'
}, {
  name: '夢桜 - Jusqu\'à Grand-Père',
  url: 'http://music.163.com/song/media/outer/url?id=756162.mp3'
}, {
  name: 'Auspicious sign - Jusqu\'à Grand-Père',
  url: 'http://music.163.com/song/media/outer/url?id=2066716812.mp3'
},{
  name: '人间の里',
  url: 'http://music.163.com/song/media/outer/url?id=852640.mp3'
},{
  name: '独坐·敬亭',
  url: 'http://music.163.com/song/media/outer/url?id=1380269382.mp3'
},{
  name: '我将在何处游荡',
  url: 'http://music.163.com/song/media/outer/url?id=2070606086.mp3'
},{
  name: 'Wish My Life Away',
  url: 'http://music.163.com/song/media/outer/url?id=529668945.mp3'
}
])

const imgUrl = 'https://tu.ltyuanfang.cn/api/fengjing.php';
const sentenceUrl = 'https://tenapi.cn/v2/yiyan';
const musicUrl = musicUrlArr.value[Math.floor(Math.random() * 3)].url

const sentence = ref('');
const initialText = ref('');
const ripples = ref('涟 漪 - 一 个 没 什 么 用 的 网 页 ')

onMounted(() => {
  axios({
    method: 'get',
    url: sentenceUrl,
    responseType: 'json'
  }).then((response) => {
    sentence.value = response.data;
    initialText.value = sentence.value;
  }).catch((error) => {
    console.error('Error fetching sentence:', error);
  });
  let banner = "%c _ \\_)         |          \n" +
      "%c   / | _ \\ _ \\ |  -_)(_-< \n" +
      "%c_|_\\_|.__/.__/_|\\___|___/ \n" +
      "%c     _|  _|               \n";
  let styles = [
    "color: #008000; font-weight: bold;",
    "color: #000080; font-weight: bold;",
    "color: #FF0000; font-weight: bold;",
    "color: #FFA500; font-weight: bold;"
  ];
  console.log.apply(console, [banner].concat(styles));
});

async function clickCopy() {
  try {
    const blob = new Blob([sentence.value], {type: 'text/plain'});
    const clipboardItem = new ClipboardItem({'text/plain': blob});
    await navigator.clipboard.write([clipboardItem]);
    alert('复制完成')
  } catch (err) {
    console.log(err)
  }
}

function isDisplayInformation(isDisplay) {
  const element = document.getElementById('information')
  if (isDisplay) {
    element.style.display = 'none'
  } else {
    element.style.display = 'block'
  }
}

document.addEventListener('mousemove', function (e) {
  let xPos = e.pageX;
  let yPos = e.pageY;
  let circleOut = document.querySelector('.circle-out');
  if (circleOut) {
    circleOut.style.top = (yPos - 100) + 'px';
    circleOut.style.left = (xPos - 100) + 'px';
  }
});

</script>

<style>
@import '@/assets/css/default.css';
@import "@/assets/css/home.css";
</style>
