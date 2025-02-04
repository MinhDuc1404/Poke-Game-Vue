<template>
  <div class="card" :class="{ disabled: IsDisabled }" :style="{
    height: `${(920 - 16 * 4) / Math.sqrt(this.CardTotal.length) - 16}px`,
    width: `${((920 - 16 * 4) / Math.sqrt(this.CardTotal.length) - 16) * 3 / 4}px`
  }">
    <div class=" card__inner" :class="{ flip: IsFlipped }" @click="OnToggleFlip">
      <div class="card__face card__face__front">
        <div class="content" :style="{
          backgroundSize: `${((920 - 16 * 4) / Math.sqrt(this.CardTotal.length) - 16) / 3}px
          ${(((920 - 16 * 4) / Math.sqrt(this.CardTotal.length) - 16) * 3 / 4) / 3}px`

        }"></div>
      </div>
      <div class="card__face card__face__back">
        <div class="content" :style="{ backgroundImage: `url(/src/assets/${ImageBackUrl})` }"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      IsFlipped: false,
      IsDisabled: false,
    };
  },
  props:
  {
    ImageBackUrl:
    {
      type: String,
      require: true,
    },
    CardTotal:
    {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  methods: {
    OnToggleFlip() {
      if (this.IsDisabled) return false;
      this.IsFlipped = !this.IsFlipped;
    },
    OnCardFlip() {
      this.IsFlipped = false;
    },
    OnDisabledCard() {
      this.IsDisabled = true;
    }
  },
};
</script>

<style scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}


.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  position: relative;
  cursor: pointer;
}

.card.disabled .card__inner {
  cursor: default;
  border: 2px solid blue;
  border-radius: 1rem;
}

.card__face {
  position: absolute;
  height: 100%;
  width: 100%;
  backface-visibility: hidden;
  border-radius: 1rem;
  overflow: hidden;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgb(0, 0, 0, 0.2);
}

.card__face__back {
  transform: rotateY(-180deg);
  background-color: var(--light);
}

.card__face__front .content {
  background-image: url("../assets/images/icon_back.png");
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
  background-size: 40px 40px;
}

.card__face__back .content {
  width: 100%;
  height: 100%;
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
}

.card__inner.flip {
  transform: rotateY(-180deg);
}

.content {
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
}
</style>
