<template>
  <div class="item__row">
    <div class="form__counter">
      <button
        type="button"
        aria-label="Убрать один товар"
        style="cursor: pointer"
        @click="decreaseAmount"
      >
        <svg width="12" height="12" fill="currentColor">
          <use xlink:href="#icon-minus"></use>
        </svg>
      </button>

      <input type="text" v-model="currentProductAmount" />

      <button
        type="button"
        aria-label="Добавить один товар"
        style="cursor: pointer"
        @click="increaseAmount"
      >
        <svg width="12" height="12" fill="currentColor">
          <use xlink:href="#icon-plus"></use>
        </svg>
      </button>
    </div>

    <button
      class="button button--primery"
      type="submit"
      :disabled="buttonDisable"
    >
      В корзину
    </button>
  </div>
</template>

<script>
export default {
  props: ["productAmount", "buttonDisable"],
  data() {
    return { currentProductAmount: this.productAmount };
  },
  methods: {
    increaseAmount() {
      this.currentProductAmount++;
      this.$emit("update:productAmount", this.currentProductAmount);
    },
    decreaseAmount() {
      if (this.currentProductAmount > 1) {
        this.currentProductAmount--;
        this.$emit("update:productAmount", this.currentProductAmount);
      }
    },
  },
};
</script>
