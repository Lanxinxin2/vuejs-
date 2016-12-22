<template>
  <div id="app">
    <v-header></v-header>
    <v-shopcar></v-shopcar>
  </div>
</template>

<script>
import vHeader from './components/head'
import vShopcar from './components/shopcar'

export default {
  name: 'app',
  components: {
    'vHeader': vHeader,
    'vShopcar': vShopcar
  }
}
</script>

<style>
  @import './assets/css/assets.css';
</style>
