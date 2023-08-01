<template>
  <ul class="catalog__list">
    <ProductItem v-for="(product, i) in products" :key="i" :product="product" />
  </ul>
</template>

<script>
import ProductItem from "@/components/ProductItem";

export default {
  components: { ProductItem },
  props: ["products"],
};
</script>
