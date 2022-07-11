<template>
  <main-screen
    v-if="settings.statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <div class="container">
    <interact-screen
      v-if="settings.statusMatch === 'interact'"
      :cardsContext="settings.cardsContext"
    />
    <footer-screen />
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import FooterScreen from "./components/FooterScreen.vue";
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
      },
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    FooterScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("On handle before start: ", config.totalOfBlock);
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
      console.log(this.settings.cardsContext);
      this.settings.statusMatch = "interact";
      this.settings.startedAt = new Date().getTime();
    },
  },
};
</script>
