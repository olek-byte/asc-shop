<template>
  <div class="catalog-notification">
    <TransitionGroup
      name="transition-animate"
      class="catalog-notification__list"
      tag="div"
    >
      <div
        class="catalog-notification__content"
        v-for="message in messages"
        :key="message.id"
        :class="message.icon"
      >
        <div class="catalog-notification__content-text">
          <span>{{ message.name }}</span>
          <img src="../../assets/images/logo/check.png" alt="check" />
        </div>
      </div>
    </TransitionGroup>
  </div>
</template>

<script>
export default {
  name: "CatalogNotification",
  props: {
    messages: {
      type: Array,
      default: () => {
        return [];
      },
    },
    rightBtn: {
      type: String,
      default: "ok",
    },
    leftBtn: {
      type: String,
      default: "",
    },
    timeout: {
      type: Number,
      default: 3000,
    },
  },
  methods: {
    hideNotification() {
      let vm = this;
      if (!this.messages.length) {
        setInterval(function () {
          vm.messages.splice(vm.messages.length - 1, 1);
        }, vm.timeout);
      }
    },
  },
  watch: {
    messages() {
      this.hideNotification();
    },
  },
  mounted() {
    this.hideNotification();
  },
};
</script>

<style lang="scss">
.catalog-notification {
  position: fixed;
  top: 80px;
  right: 16px;
  z-index: 10;
  &__list {
    display: flex;
    flex-direction: column;
  }
  &__content {
    padding: 0 10px;
    border-radius: 4px;
    color: $color-light;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 54px;
    margin-bottom: $margin * 2;
    background-color: #2c3e50;
    max-width: 200px;
    text-align: left;
    &.error {
      background-color: $color-btn;
    }
    &.warning {
      background-color: orange;
    }
    &.check_circle {
      background-color: #2c3e50;
    }
  }
  &__content-text {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: $padding * 2;
  }
}
.transition-animate {
  &-enter-from {
    transform: translateX(120px);
    opacity: 0;
  }
  &-enter-active {
    transition: all 0.6s ease;
  }

  &-leave-active {
    transition: transform 0.6s ease, opacity 0.6s, height 0.6s 0.2s;
  }
  &-leave-to {
    height: 0;
    transform: translateX(120px);
    opacity: 0;
  }
  &-move {
    transition: transform 0.6s ease;
  }
}
</style>