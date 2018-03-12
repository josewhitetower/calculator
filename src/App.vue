<template>
<div>
    <Calculator/>
    <p class="footer">Created with love by <a href="https://github.com/josewhitetower" target="_blank"> @josewhitetower</a></p>
    <p class="footer">Inspired by <a href="https://codepen.io/mjijackson/pen/xOzyGX" target="_blank">Michael Jackson's React Calculator</a></p>
    <p class="footer">Source code on  <a href="https://github.com/josewhitetower/calculator" target="_blank">Github</a></p>
</div>
</template>

<script>
import Calculator from './components/Calculator';

export default {
  name: 'App',
  components: {
    Calculator,
  },
};
</script>

<style>
.footer{
  text-align: center;
  font-family: 'Courier New', Courier, monospace;
}
</style>
