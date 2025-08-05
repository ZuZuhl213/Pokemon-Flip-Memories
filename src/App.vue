<template>
  <img class="logo" alt="Vue logo" src="./assets/images/ZuZu.png" v-if="status === 'default'">
  <main-screen 
  v-if="status === 'default'" 
  @onStart="onHandleBeforeStart($event)"/>
  <interact-screen 
  v-if="status === 'match'" 
  :cardcontext="settings.cardcontext"
  @onGameOver="onGetResult($event)" />
  <result-screen 
  v-if="status === 'over'"
  :results="results"
  @onPlayAgain="onPlayAgain"
  @onBackToMenu="onBackToMenu"/>

</template>

<script>
import MainScreen from './components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'
import ResultScreen from './components/ResultScreen.vue'
import './assets/styles/global.css'
import { shuffled } from './utils/array'

export default {
  name: 'App',
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },

  props: {
    cardcontext: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      settings: {
        totalOfBlock: 0,
        cardcontext: [],
        startedAt: null,
      },
      status: "default", // defualt, match, over
      results: null,
    }
  },

  methods: {

    onHandleBeforeStart(config) {
      console.log("Starting game with config:", config);
      
      // Lưu số lượng thẻ bài
      this.settings.totalOfBlock = config.totalOfBlock;

      // Tạo mảng ID duy nhất (số lượng = totalOfBlock/2)
      const firstCard = Array.from({ length: this.settings.totalOfBlock / 2 }, (_, i) => i + 1);
      
      // Tạo mảng đầy đủ với mỗi ID xuất hiện 2 lần
      const cards = [...firstCard, ...firstCard];
      
      console.log("Cards before shuffle:", cards);

      // Shuffle mảng và lưu vào settings
      this.settings.cardcontext = shuffled(shuffled(shuffled(cards)));

      this.settings.startedAt = new Date().getTime();

      console.log("Cards after shuffle:", this.settings.cardcontext);

      // Chuyển sang màn hình game
      this.status = "match";

    },

    onGetResult(result) {
      console.log("Game result:", result);
      // this.results = result;
      const playTime = new Date().getTime() - this.settings.startedAt;
      this.results = {
        ...result,
        playTime: playTime,
        playTimeSeconds: Math.floor(playTime / 1000),
    };
      this.status = "over"; // Quay lại
    },

    onPlayAgain() {
      const config = { totalOfBlock: this.settings.totalOfBlock };
      this.onHandleBeforeStart(config);
    },

    onBackToMenu() {
      this.status = "default";
      this.results = null;
      this.settings = {
        totalOfBlock: 0,
        cardcontext: [],
        startedAt: null,
      };
    }


  }
}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
}

.logo {
  width: 300px;
  height: auto;
  margin: 20px auto;
  display: block;
  position: absolute;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
}
</style>