<template>
  <div class="catalog-item">
    <catalog-popup
      v-if="isInfoPopupVisible"
      rightBtnPopup="Add to cart"
      :popupTitle="product_data.name"
      @rightBtnAction="addToCart"
      @closePopup="closeInfoPopup"
    >
      <div class="catalog-item__popup">
        <div class="catalog-item__image-wrap">
          <img
            class="catalog-item__image"
            :src="require('../../assets/images/' + product_data.image)"
            alt="img"
          />
        </div>
        <div class="catalog-item__popup-content">
          <p class="catalog-item__name" style="font-weight: 700">
            {{ product_data.name }}
          </p>
          <p class="catalog-item__price">
            <span style="font-weight: 700">Price:</span>
            {{ product_data.price }} UAH
          </p>
          <p class="catalog-item__price" style="font-weight: 700">
            {{ product_data.category }}
          </p>
          <p class="catalog-item__descr">
            <span style="font-weight: 700">Description:</span>
            {{ product_data.description }}
          </p>
        </div>
      </div>
    </catalog-popup>
    <div class="catalog-item__image-wrap">
      <img
        class="catalog-item__image"
        :src="require('../../assets/images/' + product_data.image)"
        alt="img"
      />
    </div>
    <div class="catalog-item__box">
      <p class="catalog-item__name">{{ product_data.name }}</p>
      <p class="catalog-item__price">Price: {{ product_data.price }} UAH</p>
      <div class="catalog-item__btns">
        <button class="catalog-item__show-info btn" @click="showPopupInfo">
          Show info
        </button>
        <button class="catalog-item__btn btn" @click="addToCart">
          Add to cart
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import CatalogPopup from "../popup/CatalogPopup.vue";
export default {
  name: "CatalogItem",
  components: {
    CatalogPopup,
  },
  props: {
    product_data: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      isInfoPopupVisible: false,
    };
  },
  methods: {
    showPopupInfo() {
      this.isInfoPopupVisible = true;
    },
    closeInfoPopup() {
      this.isInfoPopupVisible = false;
    },
    addToCart() {
      this.$emit("addToCart", this.product_data);
    },
  },
};
</script>
<style lang="scss">
.catalog-item {
  box-shadow: 0 0 8px 0 $color-shadow;
  margin: 0 20px 20px 0;
  @media (max-width: 470px) {
    padding: $padding;
  }
  &__image-wrap {
    overflow: hidden;
    max-width: 200px;
  }
  &__image {
    object-fit: cover;
    object-position: center;
    width: 100%;
    height: 100%;
    min-height: 316px;
  }
  &__box {
    padding: 10px;
  }
  &__btns {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: stretch;
    gap: $padding;
  }
  &__show-info.btn {
    background-color: $main-color;
  }
  &__btn {
    cursor: pointer;
  }
  &__popup {
    display: flex;
    justify-content: space-between;
    width: 100%;
  }
  &__popup-content {
    padding-left: 20px;
    width: 100%;
    text-align: left;
  }
}
</style>