<template>
  <li class="cart__item product">
    <div class="product__pic">
      <img
        :src="item.product.image"
        width="120"
        height="120"
        :alt="item.product.title"
      />
    </div>
    <h3 class="product__title">
      {{ item.product.title }}
    </h3>
    <span class="product__code">Артикул: {{ item.product.id }}</span>

    <div class="product__counter form__counter">
      <button
        type="button"
        aria-label="Убрать один товар"
        style="cursor: pointer"
        @click="decreaseAmount"
      >
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-minus"></use>
        </svg>
      </button>

      <input type="text" v-model.number="amount" name="count" />

      <button
        type="button"
        aria-label="Добавить один товар"
        style="cursor: pointer"
        @click="increaseAmount"
      >
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-plus"></use>
        </svg>
      </button>
    </div>

    <b class="product__price">
      {{ (item.product.price * item.amount) | numberFormat }} ₽
    </b>

    <button
      class="product__del button-del"
      type="button"
      aria-label="Удалить товар из корзины"
      style="cursor: pointer"
      @click="deleteProduct(item.productId)"
    >
      <svg width="20" height="20" fill="currentColor">
        <use xlink:href="#icon-close"></use>
      </svg>
    </button>
  </li>
</template>

<script>
import numberFormat from "@/helpers/numberFormat";
import { mapActions } from "vuex";

export default {
  props: ["item"],
  filters: { numberFormat },
  computed: {
    amount: {
      get() {
        return this.item.amount;
      },
      set(value) {
        this.updateGlobalAmount(value);
      },
    },
  },
  methods: {
    ...mapActions({ deleteProduct: "deleteCartProduct" }),

    increaseAmount() {
      this.amount++;
      this.updateGlobalAmount(this.amount + 1);
    },

    decreaseAmount() {
      if (this.amount > 1) {
        this.updateGlobalAmount(this.amount - 1);
      }
    },

    updateGlobalAmount(amount) {
      this.$store.dispatch("updateCartProductAmount", {
        productId: this.item.productId,
        amount,
      });
    },
  },
};
</script>
