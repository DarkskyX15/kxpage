<template>
  <div class="wrapper">
    <div class="layout">
      <slot></slot>
    </div>
  </div>
</template>

<style lang="css" scoped>
.wrapper {
  position: absolute;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.layout {
  position: absolute;
  height: 100%;
  width: 100vw;
  background-color: var(--kx-dark-black0);
}
.forward-enter-active.wrapper {
  z-index: -1;
}
.forward-leave-active.wrapper {
  z-index: 0;
  transition: width 1s ease-in-out;
}
.forward-leave-to.wrapper {
  width: 0vw;
}
.rewind-enter-active.wrapper {
  z-index: 1;
  transition: width 1s ease-in-out;
}
.rewind-enter-from.wrapper {
  width: 0vw;
}
</style>
