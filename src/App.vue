<template>
  <img alt="Vue logo" src="./assets/images/ZuZu.png" v-if="status === 'default'">
  <main-screen v-if="status === 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen v-if="status === 'match'"/>
</template>

<script>
import MainScreen from './components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'
import './assets/styles/global.css'

export default {
  name: 'App',
  components: {
    MainScreen,
    InteractScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlock: 0,
        cardcontext: [],
      },

      status: "default",

    }
  },
  methods: {
    onHandleBeforeStart(config) {
      // Lấy cấu hình từ component MainScreen
      // và chuyển sang trạng thái "match"
      // để
      // hi
      //ển
      // thị
      // component InteractScreen
      // console.log("running", config)
      console.log("running", config);
      this.settings.totalOfBlock = config.totalOfBlock;

      const firstCard = Array.from({ length: this.settings.totalOfBlock /2 }, (_, i) => i + 1);
      const secondCard = [...firstCard];
      const cards = [...firstCard, ...secondCard];

      console.log(cards);

      this.status = "match";
      console.log("running", config);

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
  margin-top: 60px;
}
</style>
