<template>
  <div class="window" :style="{width: windowWidth, height: windowHeight}">
    <div class="header" :style="{fontSize: faFontSize}">
        <i class="fa fa-circle"></i>
        <i class="fa fa-circle"></i>
        <i class="fa fa-circle"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: "Window",
  props: ["ppi", "windowsScaleFactor"],
  computed: {
    scalingFactor() {
        return 100 / this.ppi * this.windowsScaleFactor
    },
    faFontSize() {
        return `${10 * this.scalingFactor}px`;
    },
    windowHeight() {
        return `${60 * this.scalingFactor}px`
    },
    windowWidth() {
        return `${100 * this.scalingFactor}px`
    }
  }
};
</script>

<style>
    .window {
        background-color: #eee;
        border-radius: 4px;
        overflow: hidden;
        position: relative;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
    .header {
        height: 30%;
        background-color: #666;
    }
</style>
