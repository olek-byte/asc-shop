<template>
  <form class="catalog-search">
    <img
      src="../../assets/images/logo/loupe.png"
      class="catalog-search__icon"
      @click="searchByName(searchQuery)"
    />
    <input
      name="query"
      placeholder="Search..."
      class="catalog-search__input"
      v-model="searchQuery"
    />
    <img
      src="../../assets/images/logo/clear.png"
      class="catalog-search__clear"
      @click="clearSearch"
    />
  </form>
</template>

<script>
import { mapActions } from "vuex";

export default {
  name: "CatalogSearch",
  data() {
    return {
      searchQuery: "",
    };
  },
  methods: {
    ...mapActions(["GET_SEARCH_QUERY"]),
    searchByName(value) {
      this.GET_SEARCH_QUERY(value);
    },
    clearSearch() {
      this.searchQuery = "";
      this.GET_SEARCH_QUERY();
    },
  },
};
</script>

<style lang="scss">
.catalog-search {
  position: relative;
  &__icon,
  &__clear {
    position: relative;
    width: 30px;
    height: 29px;

    color: $main-color;
    cursor: pointer;
    &:hover {
      opacity: 0.7;
    }
  }
  &__icon {
    top: 10px;
    left: -2px;
  }
  &__clear {
    top: 9px;
    right: 25px;
  }
  &__input {
    border: 1px solid $color-border;
    margin-right: 24px;
    padding: 6px;
    width: 150px;
    height: 30px;
  }
}
</style>