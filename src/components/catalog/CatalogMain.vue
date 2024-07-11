<template>
  <div class="catalog">
    <router-link class="link" :to="{ name: 'cart' }">
      <div class="catalog__link">
        <div class="catalog__link-container">
          <img
            class="catalog__link-img"
            src="https://cdn.pixabay.com/photo/2014/04/02/17/03/shopping-cart-307772_1280.png"
            alt="cart"
          />
          <span>{{ CART.length }}</span>
        </div>
      </div>
    </router-link>
    <div class="catalog__wrapper">
      <h1>Ascendix Dress Shop Online</h1>
      <catalog-search />
    </div>
    <catalog-notification :messages="messages" />

    <div class="catalog__container">
      <div class="catalog__filters">
        <catalog-select
          :options="categories"
          @select="sortByCategories"
          :selected="selected"
        />

        <div class="catalog__range">
          <input
            type="range"
            min="980"
            max="1900"
            step="10"
            v-model.number="minPrice"
            @change="setRangeSlider"
          />
          <input
            type="range"
            min="980"
            max="1900"
            step="10"
            v-model.number="maxPrice"
            @change="setRangeSlider"
          />
        </div>
        <div class="catalog__range-values">
          <p>Min: {{ minPrice }} UAH</p>
          <p>Max: {{ maxPrice }} UAH</p>
        </div>
      </div>

      <div class="catalog__list">
        <catalog-item
          v-for="product in filteredProducts"
          :key="product.article"
          :product_data="product"
          @addToCart="addToCart"
        />
      </div>
    </div>
  </div>
</template>

<script>
import CatalogItem from "./CatalogItem.vue";
import CatalogSelect from "./CatalogSelect.vue";
import CatalogNotification from "../notifications/CatalogNotification.vue";
import CatalogSearch from "./CatalogSearch.vue";
import { mapActions, mapGetters } from "vuex";
export default {
  name: "CatalogMain",
  components: {
    CatalogItem,
    CatalogSelect,
    CatalogNotification,
    CatalogSearch,
  },
  data() {
    return {
      categories: [
        { name: "All", value: "All" },
        { name: "Male", value: "M" },
        { name: "Female", value: "F" },
      ],
      selected: "All",
      sortedProducts: [],
      minPrice: 980,
      maxPrice: 1900,
      messages: [],
    };
  },
  computed: {
    ...mapGetters(["PRODUCTS", "CART", "SEARCH_QUERY"]),
    filteredProducts() {
      let vm = this;
      if (this.sortedProducts.length) {
        return this.sortedProducts;
      } else {
        vm.maxPrice = 1900;
        vm.minPrice = 890;
        vm.selected = "All";
        return this.PRODUCTS;
      }
    },
  },
  methods: {
    ...mapActions(["GET_PRODUCTS_FROM_API", "ADD_TO_CART"]),
    addToCart(data) {
      this.ADD_TO_CART(data).then(() => {
        let timeStamp = Date.now().toLocaleString();
        this.messages.unshift({
          name: "The product has been added to the cart",
          icon: "check_circle",
          id: timeStamp,
        });
      });
    },
    sortByCategories(category) {
      let a = this;
      this.sortedProducts = [...this.PRODUCTS];
      this.sortedProducts = this.sortedProducts.filter(function (item) {
        return item.price >= a.minPrice && item.price <= a.maxPrice;
      });
      if (category) {
        this.sortedProducts = this.sortedProducts.filter(function (e) {
          a.selected = category.name;
          return e.category === category.name;
        });
      }
    },
    setRangeSlider() {
      if (this.minPrice > this.maxPrice) {
        let stop = this.maxPrice;
        this.maxPrice = this.minPrice;
        this.minPrice = stop;
      }
      this.sortByCategories();
      this.selected = "All";
    },
    filteredByName(value) {
      this.sortedProducts = [...this.PRODUCTS];
      this.selected = "All";
      if (value) {
        this.sortedProducts = this.sortedProducts.filter(function (item) {
          return item.name.toLowerCase().includes(value.toLowerCase());
        });
      } else {
        this.sortedProducts = this.PRODUCTS;
      }
    },
  },
  watch: {
    SEARCH_QUERY() {
      this.filteredByName(this.SEARCH_QUERY);
    },
  },
  mounted() {
    this.GET_PRODUCTS_FROM_API();
    this.filteredByName(this.SEARCH_QUERY);
  },
};
</script>
<style lang="scss">
.catalog {
  padding-top: 30px;
  &__wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;

    @media (max-width: 660px) {
      margin-bottom: 50px;
      flex-wrap: wrap;
    }
  }
  &__container {
    display: flex;
    justify-content: start;
    align-items: start;
  }
  @media (max-width: 470px) {
  }
  &__list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
  }
  &__link {
    position: fixed;
    top: 10px;
    right: 10px;
    width: 70px;
    height: 50px;
    border: solid 1px $color-border;
    background-color: #fff;
    cursor: pointer;

    &-container {
      position: relative;
      left: -5px;
      & span {
        position: absolute;
        top: -9px;
        right: -9px;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 20px;
        height: 20px;
        background: #ccc;
        color: #000;
        border-radius: 50%;
      }
    }
    &-img {
      width: 30px;
      position: absolute;
      right: 18px;
      top: 10px;
    }
  }
  &__filters {
    display: flex;
    flex-direction: column;
    justify-content: start;
    align-items: start;
    min-width: 200px;
  }
  &__range {
    width: 150px;
    text-align: center;
    position: relative;
    @media (max-width: 470px) {
      width: 100px;
    }
    & svg,
    & input[type="range"] {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
    }
  }
  &__range-values {
    min-width: 80px;
    text-align: left;
  }
}
input[type="range"]::-webkit-slider-thumb {
  z-index: 2;
  position: relative;
  top: 2px;
  margin-top: -7px;
}
</style>