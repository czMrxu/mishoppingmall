<template>
  <div>
    <nav-header></nav-header>
    <router-view></router-view>
    <nav-footer></nav-footer>
  </div>
</template>

<script>
  import NavHeader from '../components/NavHeader.vue'
  import NavFooter from '../components/NavFooter.vue'
  export default {
    name: 'nav-home',

    components: {
      NavHeader,
      NavFooter
    },

    methods: {}
  }
</script>

<style scoped>
</style>