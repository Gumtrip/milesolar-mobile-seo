<template>
  <div>
    <Header></Header>
    <nuxt />
    <Footer></Footer>
  </div>
</template>

<script>
  import Header from '@/components/Header.vue'
  import Footer from '@/components/Footer.vue'
  export default {
    components: {
      Footer, Header
    }
  }
</script>
<style lang="scss">
</style>
