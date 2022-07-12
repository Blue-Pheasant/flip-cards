<template>
  <main-screen
    v-if="settings.statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <div class="container">
    <interact-screen
      v-if="settings.statusMatch === 'interact'"
      :cardsContext="settings.cardsContext"
      @onFinish="getResult()"
    />
    <result-screen
      v-if="settings.statusMatch === 'finish'"
      :totalTime="settings.totalTime"
      @onStartAgain="startAgain()"
    />
    <footer-screen />
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import FooterScreen from "./components/FooterScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
  data() {
    return {
      settings: {
        totalBlocks: 0,
        statusMatch: "default",
        cardsContext: [],
        startedAt: null,
        totalTime: null,
      },
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    FooterScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      // console.log("On handle before start: ", config.totalOfBlock);
      this.settings.totalBlocks = config.totalOfBlock;
      const firstCards = Array.from(
        { length: this.settings.totalBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      // console.log(this.settings.cardsContext);
      this.settings.statusMatch = "interact";
      this.settings.startedAt = new Date().getTime();
    },
    getResult() {
      this.settings.totalTime = new Date().getTime() - this.settings.startedAt;
      this.settings.statusMatch = "finish";
    },
    startAgain() {
      this.settings.statusMatch = "default";
    },
  },
};
</script>
