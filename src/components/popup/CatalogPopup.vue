<template>
  <div class="catalog-popup__wrapper" ref="popup_wrapper">
    <div class="catalog-popup">
      <div class="catalog-popup__content">
        <slot></slot>
      </div>
      <div class="catalog-popup__footer">
        <button class="btn catalog-popup__close" @click="closePopup">
          Close
        </button>
        <button class="catalog-popup__btn btn" @click="rightBtnAction">
          {{ rightBtnPopup }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CatalogPopup",
  props: {
    rightBtnPopup: {
      type: String,
      default: "Ok",
    },
    popupTitle: {
      type: String,
      default: "Popup name",
    },
  },
  methods: {
    rightBtnAction() {
      this.$emit("rightBtnAction");
    },
    closePopup() {
      this.$emit("closePopup");
    },
  },
  mounted() {
    let vm = this;
    document.addEventListener("click", function (item) {
      if (item.target === vm.$refs["popup_wrapper"]) {
        vm.closePopup();
      }
    });
  },
};
</script>

<style lang="scss">
.catalog-popup {
  padding: $padding * 2;
  position: fixed;
  top: 250px;
  width: 450px;
  box-shadow: 0 0 $padding * 2 0 $color-shadow;
  left: calc(50% - 200px);
  z-index: 333;
  background-color: $color-light;
  @media (max-width: 768px) {
    width: 350px;
    left: calc(50% - 166px);
  }
  @media (max-width: 448px) {
    width: 280px;
    left: calc(50% - 146px);
  }
  &__wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: $color-popup;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    z-index: 433;
    min-height: 220vh;
    & body {
      overflow: hidden;
    }
  }
  &__footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 16px;
  }
  &__content {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  &__icon {
    cursor: pointer;
    &:hover {
      opacity: 0.8;
    }
  }
  &__btn {
    padding: $padding;
    color: $color-light;
    background-color: $color-special;
  }
  &__close.btn {
    padding: $padding;
    color: $color-light;
    background-color: $color-btn;
    &:hover {
      opacity: 0.8;
    }
  }
}
</style>