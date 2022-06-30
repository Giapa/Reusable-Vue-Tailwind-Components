<template>
  <button
    role="button"
    type="button"
    :class="['base-button', {'base-button--bordered': bordered}]"
    @click="$emit('click')"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "BaseButton",
  props: {
    bordered: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
.base-button {
  font-size: 1em;
  cursor: pointer;
  color: white;
  &:focus {
    outline: 2px solid transparent;
    outline-offset: 2px;
  }
  &--bordered {
    border: 2px solid black;
  }
}
</style>
