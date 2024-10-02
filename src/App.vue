<template>
  <div style="position: relative; max-width: 100%;
  height: 100vh; overflow: hidden">
    <Typewriter class="typewrite" @click="clickCopy" v-if="initialText" :text="initialText"
                style="cursor: pointer; display:block;position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); z-index: 100"/>
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
    <div class="footer" style="position: absolute; top: 90%; left: 50%; transform: translate(-50%, -50%);">
      <a href="https://github.com/s0uths1de">
        <Typewriter style="font-size: 12px;" text="S1de个人仓库" :is-hover="false"/>
      </a>
      <br>
      <br>
      <a href="https://beian.miit.gov.cn/">
        <Typewriter style="font-size: 12px;" text="赣ICP备2024033384号" :is-hover="false"/>
      </a>
    </div>
  </div>


  <MusicPlayer :musicUrl="musicUrl"/>
</template>

<script setup>
import {ref, onMounted} from 'vue';
import Typewriter from './components/Typewriter.vue';
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
}
])

const imgUrl = 'https://tu.ltyuanfang.cn/api/fengjing.php';
const sentenceUrl = 'https://tenapi.cn/v2/yiyan';
const musicUrl = musicUrlArr.value[Math.floor(Math.random() * 3)].url

const sentence = ref('');
const initialText = ref('');
const ripples = ref('涟 漪 - 一 个 没 什 么 用 的 网 页')

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

</script>

<style>
@import '@/assets/css/default.css';

.footer {
  width: 65%;
  height: 3em;
  line-height: 1em;
  color: white;
  margin: 20px auto;
  text-align: center;
}

.introduce {
  position: absolute;
  top: 7%;
  left: 50%;
  transform: translate(-50%, -50%);
  height: 50px;
  text-align: center;
  cursor: pointer;
}

.information{
  position: absolute;
  margin-top: 40px;
  left: 50%;
  transform: translate(-50%, -50%);
  height: 50px;
  text-align: center;
  width: 150px;
  color: white;
  font-size: 18px;
  background: rgba(255, 255, 255, .5);
  backdrop-filter: blur(3px);
  padding: 0 15px;
  border-radius: 20px;
  display: none;
}

.information:hover {
  box-shadow: rgba(0, 0, 0, 0.07) 0px 1px 2px, rgba(0, 0, 0, 0.07) 0px 2px 4px, rgba(0, 0, 0, 0.07) 0px 4px 8px, rgba(0, 0, 0, 0.07) 0px 8px 16px, rgba(0, 0, 0, 0.07) 0px 16px 32px, rgba(0, 0, 0, 0.07) 0px 32px 64px;
}

.information-button {
  width: 50px;
  height: 35px;
  margin-top: 7px;
  line-height: 35px;
  border-radius: 15px;
  cursor: pointer;
}

.information-button:hover{
  background: rgba(0,0,0,0.4);
}
</style>
