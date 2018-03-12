<template>
   <div
      ref="autoshrink"
      :style="{ transform: `scale(${scale},${scale})` }" >
      {{value}}</div>
</template>

<script>
export default {
  props: ['value'],
  data() {
    return {
      scale: 1,
      full: false,
    };
  },
  updated() {
    const offsetWidth = this.$refs.autoshrink.offsetWidth;
    const parentNode = this.$refs.autoshrink.parentNode.offsetWidth;
    const scale = offsetWidth / parentNode;
    if (scale > 1) {
      this.scale = 1 / scale;
      this.full = true;
    } else if (this.scale !== 1) {
      this.scale = 1;
    }
    this.$emit('scale', scale);
  },
};
</script>

<style>

/* .calculator-screen {
    justify-content: center;
}
.full {
  justify-content: center;
} */
</style>
