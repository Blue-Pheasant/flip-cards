<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :cardsContext="cardsContext"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :rules="rules"
        @onFlip="check($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip: CardFlip,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    check(card) {
      console.log(card);
      this.rules.push(card);
      if (this.rules.length === 2) {
        if (this.rules[0].value === this.rules[1].value) {
          console.log("right");
          this.$refs[`card-${this.rules[0].index}`].onEnabledDisabledMode();
          this.$refs[`card-${this.rules[1].index}`].onEnabledDisabledMode();
        } else {
          console.log("wrong");
          setTimeout(() => {
            this.$refs[`card-${this.rules[0].index}`].onFlipBackCard();
            this.$refs[`card-${this.rules[1].index}`].onFlipBackCard();
            this.rules = [];
          }, 800);
        }
      }
    },
  },
};
</script>

<style scoped>
.screen {
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  width: calc(424px);
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
