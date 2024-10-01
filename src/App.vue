<template>
  <div style="position: relative; width: 100%; height: 500px;">
    <Typewriter class="typewrite" @click="clickCopy" v-if="initialText" :text="initialText"
                style=" display:block;position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); z-index: 100"/>
        <img style="width: 100%; height: 100%;" :src="imgUrl" alt="Scenic Image"/>
    <div class="introduce">
      <Typewriter v-if="initialText" :text="ripples"
                  style="color:white;font-size: 18px"/>
    </div>
  </div>

  <div class="footer">
    <a href="https://github.com/s0uths1de">
      S1de个人仓库
    </a>
    <br>
    <br>
    <a href="https://beian.miit.gov.cn/">
      赣ICP备2024033384号
    </a>
  </div>
  <MusicPlayer :musicUrl="musicUrl" />
</template>

<script setup>
import {ref, onMounted} from 'vue';
import Typewriter from './components/Typewriter.vue';
import MusicPlayer from "@/components/MusicPlayer.vue";
import axios from 'axios';

const imgUrl = 'https://tu.ltyuanfang.cn/api/fengjing.php';
const sentenceUrl = 'https://tenapi.cn/v2/yiyan';
const musicUrl = 'http://music.163.com/song/media/outer/url?id=756141.mp3'

const sentence = ref('');
const initialText = ref('');
const ripples = ref('涟 漪 - 一 个 没什 么 用 的 网 页')

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
  text-align: center
}
</style>
