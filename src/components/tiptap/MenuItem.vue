<template>
  <div class="menu-item" :class="{ 'is-active': isActive ? isActive() : null }" @click="action" :title="title">
    <svg class="remix">
      <use :xlink:href="require('./menuBar.svg') + `#ri-${icon}`" />
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    icon: {
      type: String,
    },

    title: {
      type: String,
    },

    action: {
      type: Function,
      required: false,
      default: () => {},
    },

    isActive: {
      type: Function,
      default: null,
    },

    type: {
      type: String,
    },
  },
};
</script>

<style lang="scss" scoped>
.menu-item {
  width: 1.75rem;
  height: 1.75rem;
  color: #0d0d0d;
  border: none;
  background-color: transparent;
  border-radius: 0.4rem;
  padding: 0.25rem;
  margin-right: 0.25rem;
  display: flex;

  svg {
    width: 100%;
    height: 100%;
    fill: currentColor;
  }

  &.is-active,
  &:hover {
    color: #fff;
    background-color: #0d0d0d;
  }

  .el-icon-picture {
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-size: 20px;
  }
}
</style>
