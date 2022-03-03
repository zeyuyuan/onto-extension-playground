<template>
  <div class="layout">
    <div>
      <slot name="menu"></slot>
    </div>
    <div class="layout__right">
      <slot></slot>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.layout {
  height: 100vh;
  display: flex;
  padding: 20px;
  box-sizing: border-box;
}

.layout__right {
  margin-left: 20px;
  flex: 1;
  min-height: 0;
  overflow: auto;
}
</style>
