<template>
  <main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link
            :to="{ name: 'main' }"
            class="breadcrumbs__link"
            href="index.html"
          >
            Каталог
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <router-link
            :to="{ name: 'cart' }"
            class="breadcrumbs__link"
            href="cart.html"
          >
            Корзина
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link"> Оформление заказа </a>
        </li>
      </ul>

      <h1 class="content__title">Корзина</h1>
      <span class="content__info"> {{ totalAmount }} товара </span>
    </div>

    <section class="cart">
      <form
        class="cart__form form"
        action="#"
        method="POST"
        @submit.prevent="order"
      >
        <div class="cart__field">
          <div class="cart__data">
            <BaseFormText
              title="ФИО"
              :error="formError.name"
              placeholder="Введите ваше полное имя"
              v-model="formData.name"
            />

            <BaseFormText
              title="Адрес доставки"
              :error="formError.address"
              placeholder="Введите ваш адрес"
              v-model="formData.address"
            />

            <BaseFormText
              title="Телефон"
              :error="formError.phone"
              placeholder="Введите ваш телефон"
              v-model="formData.phone"
              type="tel"
            />

            <BaseFormText
              title="Email"
              :error="formError.email"
              placeholder="Введите ваш Email"
              v-model="formData.email"
            />

            <BaseFormTextarea
              title="Комментарий к заказу"
              :error="formError.comment"
              v-model="formData.comment"
              placeholder="Ваши пожелания"
            />
          </div>

          <!-- <div class="cart__options">
            <h3 class="cart__title">Доставка</h3>
            <ul class="cart__options options">
              <li class="options__item">
                <label class="options__label">
                  <input
                    class="options__radio sr-only"
                    type="radio"
                    name="delivery"
                    value="0"
                    checked=""
                  />
                  <span class="options__value">
                    Самовывоз <b>бесплатно</b>
                  </span>
                </label>
              </li>
              <li class="options__item">
                <label class="options__label">
                  <input
                    class="options__radio sr-only"
                    type="radio"
                    name="delivery"
                    value="500"
                  />
                  <span class="options__value"> Курьером <b>500 ₽</b> </span>
                </label>
              </li>
            </ul>

            <h3 class="cart__title">Оплата</h3>
            <ul class="cart__options options">
              <li class="options__item">
                <label class="options__label">
                  <input
                    class="options__radio sr-only"
                    type="radio"
                    name="pay"
                    value="card"
                  />
                  <span class="options__value"> Картой при получении </span>
                </label>
              </li>
              <li class="options__item">
                <label class="options__label">
                  <input
                    class="options__radio sr-only"
                    type="radio"
                    name="pay"
                    value="cash"
                  />
                  <span class="options__value"> Наличными при получении </span>
                </label>
              </li>
            </ul>
          </div> -->
        </div>

        <div class="cart__block">
          <ul class="cart__orders">
            <li
              class="cart__order"
              v-for="item in products"
              :key="item.productId"
            >
              <h3>{{ item.product.title }}</h3>
              <b>{{ item.amount }} шт</b>
              <span>Артикул: {{ item.product.id }}</span>
              <b>{{ (item.product.price * item.amount) | numberFormat }} ₽</b>
            </li>
          </ul>

          <div class="cart__total">
            <p>
              Итого: <b>{{ totalAmount }}</b> товара на сумму
              <b>{{ totalPrice | numberFormat }} ₽</b>
            </p>
          </div>

          <button class="cart__button button button--primery" type="submit">
            Оформить заказ
          </button>
        </div>
        <svg
          xmlns:svg="http://www.w3.org/2000/svg"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          version="1.0"
          width="20px"
          height="20px"
          viewBox="0 0 128 128"
          xml:space="preserve"
          style="justify-self: center"
          v-if="formSending"
        >
          <rect x="0" y="0" width="100%" height="100%" fill="#FFFFFF" />
          <g>
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#000000"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#e5e5e5"
              transform="rotate(30 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#e5e5e5"
              transform="rotate(60 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#e5e5e5"
              transform="rotate(90 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#cecece"
              transform="rotate(120 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#b7b7b7"
              transform="rotate(150 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#9f9f9f"
              transform="rotate(180 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#898989"
              transform="rotate(210 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#727272"
              transform="rotate(240 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#5c5c5c"
              transform="rotate(270 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#444444"
              transform="rotate(300 64 64)"
            />
            <path
              d="M97.63 8.23a7.38 7.38 0 0 1 2.7 10.07L89.2 37.57a7.38 7.38 0 1 1-12.77-7.37l11.12-19.27a7.38 7.38 0 0 1 10.08-2.7z"
              fill="#2e2e2e"
              transform="rotate(330 64 64)"
            />
            <animateTransform
              attributeName="transform"
              type="rotate"
              values="0 64 64;30 64 64;60 64 64;90 64 64;120 64 64;150 64 64;180 64 64;210 64 64;240 64 64;270 64 64;300 64 64;330 64 64"
              calcMode="discrete"
              dur="720ms"
              repeatCount="indefinite"
            ></animateTransform>
          </g>
        </svg>
        <div class="cart__error form__error-block" v-if="formErrorMessage">
          <h4>Заявка не отправлена!</h4>
          <p>{{ formErrorMessage }}</p>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
import BaseFormText from "@/components/BaseFormText.vue";
import BaseFormTextarea from "@/components/BaseFormTextarea.vue";
import { mapGetters, mapMutations } from "vuex";
import numberFormat from "@/helpers/numberFormat";
import axios from "axios";
import { API_BASE_URL } from "@/config";

export default {
  components: { BaseFormText, BaseFormTextarea },
  data() {
    return {
      formData: {},
      formError: {},
      formErrorMessage: "",
      formSending: false,
    };
  },
  computed: {
    ...mapGetters({
      products: "cartDetailProducts",
      totalPrice: "cartTotalPrice",
      totalAmount: "cartTotalAmount",
    }),
  },
  filters: { numberFormat },
  methods: {
    ...mapMutations(["resetCart", "updateOrderInfo"]),

    order() {
      this.formError = {};
      this.formErrorMessage = "";
      this.formSending = true;

      axios
        .post(
          `${API_BASE_URL}/api/orders`,
          {
            ...this.formData,
          },
          {
            params: {
              userAccessKey: this.$store.state.userAccessKey,
            },
          }
        )
        .then((response) => {
          this.resetCart();
          this.updateOrderInfo(response.data);
          this.$router.push({
            name: "orderInfo",
            params: { id: response.data.id },
          });
        })
        .catch((error) => {
          this.formError = error.response.data.error.request || {};
          this.formErrorMessage = error.response.data.error.message;
        })
        .finally(() => (this.formSending = false));
    },
  },
};
</script>
