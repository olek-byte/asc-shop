<template>
  <div class="catalog-select">
    <p
      class="catalog-select__title"
      @click="areOptionsVisible = !areOptionsVisible"
    >
      {{ selected }}
    </p>
    <div class="catalog-select__options" v-if="areOptionsVisible">
      <p
        class="catalog-select__options-item"
        v-for="option in options"
        :key="option.value"
        @click="selectOption(option)"
      >
        {{ option.name }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "CatalogSelect",
  props: {
    options: {
      type: Array,
      default() {
        return [];
      },
    },
    selected: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      areOptionsVisible: false,
    };
  },
  methods: {
    selectOption(option) {
      this.$emit("select", option);
      this.areOptionsVisible = false;
    },
    hideSelect() {
      this.areOptionsVisible = false;
    },
  },
  mounted() {
    document.addEventListener("click", this.hideSelect.bind(this), true);
  },
  unmounted() {
    document.removeEventListener("click", this.hideSelect);
  },
};
</script>
<style lang="scss">
.catalog-select {
  position: relative;
  width: 150px;
  cursor: pointer;
  margin-bottom: 60px;
  @media (max-width: 348px) {
    margin: 0 auto;
  }
  &__title {
    border: solid 1px $color-border;
    padding: 6px;
  }
  &__options-item {
    margin: 0;
    padding: 6px;
    &:hover {
      background-color: $color-shadow;
    }
  }
  &__options {
    border: solid 1px $color-border;
    position: absolute;
    top: 43px;
    right: 0;
    width: 150px;
    background-color: $color-light;
    text-align: left;
    z-index: 100;
  }
}
</style>