<template>
  <ul class="catalog__list" v-if="products.length">
    <ProductItem
      v-for="product in products"
      :key="product.id"
      :product="product"
      @goToPage="
        (pageName, pageParams) => $emit('goToPage', pageName, pageParams)
      "
    />
  </ul>
  <div v-else>Нет подходящих товаров...</div>
</template>

<script>
import ProductItem from "@/components/ProductItem";

export default {
  components: { ProductItem },
  props: ["products"],
};
</script>
