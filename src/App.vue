<template>
  <div id="app">
    <header>
      <a href="#">
        <img
          src="https://ascendix.com/wp-content/uploads/2021/05/Header-Logo.svg"
          alt="logo"
        />
      </a>
    </header>
    <catalog-wrapper />
  </div>
</template>

<script>
import CatalogWrapper from "./components/catalog/CatalogWrapper.vue";

export default {
  name: "App",
  components: {
    CatalogWrapper,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

header {
  display: flex;
  justify-content: start;
  align-items: center;
  padding-left: 14px;
  height: 70px;
  background-color: rgb(90, 234, 253);
}
</style>
