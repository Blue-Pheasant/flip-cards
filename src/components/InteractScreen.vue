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
        :ref="`card__${index}`"
        :cardsContext="cardsContext"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :rules="rules"
        @onFlip="checkRule($event)"
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
    checkRule(card) {
      console.log(this.rules.length);
      if (this.rules.length === 2) {
        setTimeout(() => {
          this.$refs[`card__${card.index}`][0].onFlipBackCard();
        }, 800);
        return;
      }
      if (this.$refs[`card__${card.index}`][0].statusCard()) return;
      console.log(card);
      if (this.rules.length === 0) {
        this.rules.push(card);
        this.$refs[`card__${this.rules[0].index}`][0].onAbledMode();
        return;
      }
      if (this.rules.length === 1) {
        if (card.index !== this.rules[0].index) {
          this.rules.push(card);
          this.$refs[`card__${this.rules[1].index}`][0].onAbledMode();
        }
      }
      if (this.rules.length === 2) {
        if (this.rules[0].value === this.rules[1].value) {
          console.log("Right");
          this.rules = [];
        } else {
          console.log("Wrong");
          this.$refs[`card__${this.rules[0].index}`][0].disAbledMode();
          this.$refs[`card__${this.rules[1].index}`][0].disAbledMode();
          setTimeout(() => {
            // this.$refs[`card__${this.rules[0].index}`] will return a proxy list
            this.$refs[`card__${this.rules[0].index}`][0].onFlipBackCard();
            this.$refs[`card__${this.rules[1].index}`][0].onFlipBackCard();
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
