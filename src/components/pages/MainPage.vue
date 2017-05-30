<template lang="html">
  <div class="main-page columns is-gapless is-mobile">
    <div class="column is-8" style="position: relative;">
      <the-map></the-map>
    </div>
    <div class="column is-4">
      <the-list></the-list>
    </div>
  </div>
</template>

<script>
  import Map from '@/components/atomic/Maps'
  import PersonList from '@/components/atomic/PersonList'

  export default {
    name: 'mainPage',
    components: {
      'the-map': Map,
      'the-list': PersonList
    }
  }
</script>

<style lang="scss">
  .main-page {
    height: 100vh;
    width: 100%;
  }
</style>
