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
      <form class="cart__form form" action="#" method="POST">
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
              type="email"
            />

            <BaseFormTextarea
              title="Комментарий к заказу"
              :error="formError.comments"
              v-model="formData.comments"
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
            <!-- <li class="cart__order">
              <h3>Гироскутер Razor Hovertrax 2.0ii</h3>
              <b>4 990 ₽</b>
              <span>Артикул: 150030</span>
            </li>
            <li class="cart__order">
              <h3>Электрический дрифт-карт Razor Lil’ Crazy</h3>
              <b>8 990 ₽</b>
              <span>Артикул: 150030</span>
            </li> -->
          </ul>

          <div class="cart__total">
            <!-- <p>Доставка: <b>500 ₽</b></p> -->
            <p>
              Итого: <b>{{ totalAmount }}</b> товара на сумму
              <b>{{ totalPrice | numberFormat }} ₽</b>
            </p>
          </div>

          <button class="cart__button button button--primery" type="submit">
            Оформить заказ
          </button>
        </div>
        <div class="cart__error form__error-block">
          <h4>Заявка не отправлена!</h4>
          <p>
            Похоже произошла ошибка. Попробуйте отправить снова или
            перезагрузите страницу.
          </p>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
import BaseFormText from "@/components/BaseFormText.vue";
import BaseFormTextarea from "@/components/BaseFormTextarea.vue";
import { mapGetters } from "vuex";
import numberFormat from "@/helpers/numberFormat";

export default {
  components: { BaseFormText, BaseFormTextarea },
  data() {
    return {
      formData: {},
      formError: {},
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
};
</script>
