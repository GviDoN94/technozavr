<template>
  <main class="content container" v-if="!orderInfo">
    <LoadingSpinner />
  </main>
  <main class="content container" v-else>
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link
            :to="{ name: 'main' }"
            class="breadcrumbs__link"
            href="#"
          >
            Каталог
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <router-link
            :to="{ name: 'cart' }"
            class="breadcrumbs__link"
            href="#"
          >
            Корзина
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link"> Оформление заказа </a>
        </li>
      </ul>

      <h1 class="content__title">
        Заказ оформлен <span>№ {{ orderInfo.id }}</span>
      </h1>
    </div>

    <section class="cart">
      <form class="cart__form form" action="#" method="POST">
        <div class="cart__field">
          <p class="cart__message">
            Благодарим за&nbsp;выбор нашего магазина. На&nbsp;Вашу почту придет
            письмо с&nbsp;деталями заказа. Наши менеджеры свяжутся с&nbsp;Вами
            в&nbsp;течение часа для уточнения деталей доставки.
          </p>

          <ul class="dictionary">
            <li class="dictionary__item">
              <span class="dictionary__key"> Получатель </span>
              <span class="dictionary__value">
                {{ orderInfo.name }}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key"> Адрес доставки </span>
              <span class="dictionary__value">
                {{ orderInfo.address }}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key"> Телефон </span>
              <span class="dictionary__value"> {{ orderInfo.phone }} </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key"> Email </span>
              <span class="dictionary__value"> {{ orderInfo.email }} </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key"> Способ оплаты </span>
              <span class="dictionary__value"> картой при получении </span>
            </li>
          </ul>
        </div>

        <div class="cart__block">
          <ul class="cart__orders">
            <li
              class="cart__order"
              v-for="item in orderInfo.basket.items"
              :key="item.id"
            >
              <h3>{{ item.product.title }}</h3>
              <b>{{ item.quantity }} шт</b>
              <span>Артикул: {{ item.product.id }}</span>
              <b>{{ (item.price * item.quantity) | numberFormat }} ₽</b>
            </li>
          </ul>

          <div class="cart__total">
            <p>
              Итого: <b>{{ orderTotalAmount }}</b> товара на сумму
              <b>{{ orderInfo.totalPrice | numberFormat }} ₽</b>
            </p>
          </div>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
import { mapActions } from "vuex";
import numberFormat from "@/helpers/numberFormat";
import LoadingSpinner from "@/components/LoadingSpinner.vue";

export default {
  components: { LoadingSpinner },
  computed: {
    orderInfo() {
      if (!this.$store.state.orderInfo) {
        return;
      }
      return { ...this.$store.state.orderInfo };
    },

    orderTotalAmount() {
      if (!this.orderInfo) {
        return 0;
      }

      return this.orderInfo.basket.items.reduce(
        (acc, item) => acc + item.quantity,
        0
      );
    },
  },
  methods: {
    ...mapActions(["loadOrderInfo"]),
  },
  created() {
    if (this.orderInfo && this.orderInfo.id === this.$route.params.id) {
      return;
    }
    this.loadOrderInfo(this.$route.params.id);
  },
  filters: { numberFormat },
};
</script>
