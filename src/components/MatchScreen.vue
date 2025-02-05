<template>
  <div class="screen"
    :style="{ height: `${(((920 - 16 * 4) / Math.sqrt(this.Card.length) - 16) + 16 * 4) * Math.sqrt(this.Card.length)}px` }">
    <div class="screen__inner" :style="{
      width: `${(((920 - 16 * 4) / Math.sqrt(this.Card.length) - 16) * 3 / 4 + 16) * Math.sqrt(this.Card.length)}px`
    }">
      <card-flip v-for="(card, index) in Card" :key="index" :ImageBackUrl="`images/${card}.png`" :ref="`card-${index}`"
        @click="CheckRule(card, index)" :CardTotal="this.Card" />
    </div>

  </div>

</template>

<script>
import CardFlip from "./CardFlip.vue";
export default {
  components: {
    CardFlip,
  },
  data() {
    return {
      Rule: [],
      IsTrue: false,
    }
  },
  props:
  {
    Card: {
      type: Array,
      default: function () {
        return []
      }
    },
  },
  methods:
  {
    CheckRule(card, index) {

      if (this.Rule.length === 2) return false;

      if (this.$refs[`card-${index}`][0].IsDisabled == true) return false;

      this.Rule.push({ card, index });

      if (this.Rule[0].index === this.Rule[1].index) this.Rule = [];

      console.log(this.Rule)
      if (this.Rule.length === 2 && this.Rule[0].card === this.Rule[1].card) {
        const index1 = this.Rule[0].index;
        const index2 = this.Rule[1].index;
        this.$refs[`card-${index1}`][0].OnDisabledCard();
        this.$refs[`card-${index2}`][0].OnDisabledCard();
        this.Rule = [];
        const disabledEl = document.querySelectorAll('.card.disabled');
        if (disabledEl.length === this.Card.length - 2) {
          setTimeout(() => {
            this.$emit("OnFinishMatch")
          }, 920)

        }
        console.log(disabledEl)
      }
      else if (this.Rule.length === 2 && this.Rule[0].card != this.Rule[1].card) {
        const index1 = this.Rule[0].index;
        const index2 = this.Rule[1].index;
        setTimeout(() => {
          this.$refs[`card-${index1}`][0].OnCardFlip();
          this.$refs[`card-${index2}`][0].OnCardFlip();

        }, 600)
        this.Rule = [];
        console.log("Wrong...")
        return false;

      }
      else {
        return false;
      }
    }
  }
};
</script>
<style scoped>
.screen {
  width: 100%;
  height: 100%;
  background-color: var(--dark);
  color: var(--light);
  z-index: 2;
  position: absolute;
  top: 0;
  left: 0;
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
