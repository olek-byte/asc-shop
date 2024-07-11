<template>
  <div class="catalog-cart">
    <router-link class="link" :to="{ name: 'catalog' }">
      <div class="catalog__link">
        <!-- Back to catalog -->
        <img
          class="catalog-cart__img"
          src="../../assets/images/logo/back.png"
          alt="back"
        />
      </div>
    </router-link>
    <h1>Cart</h1>
    <p v-if="!CART.length">There are no products in cart...</p>
    <cart-item-wrapper v-if="CART.length" :cart_data="CART" />
  </div>
</template>

<script>
import CartItemWrapper from "./CartItemWrapper.vue";
import { mapGetters } from "vuex";
export default {
  name: "CatalogCart",
  components: {
    CartItemWrapper,
  },
  computed: {
    ...mapGetters(["CART"]),
  },
};
</script>

<style lang="scss">
.catalog-cart {
  padding: 30px 0 100px;
  height: inherit;
  &__img {
    width: 40px;
    margin: 4px 2px 0;
  }
  @media (max-width: 470px) {
    padding-top: 80px;
  }
}
</style>