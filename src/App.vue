<template lang="pug">
  #app
    pm-header

    router-view

    pm-footer
</template>

<script>
import PmFooter from '@/components/layout/Footer'
import PmHeader from '@/components/layout/Header'

export default {
  name: 'app',

  components: { PmFooter, PmHeader },

  data () {
    return {
    }
  }
}
</script>

<style lang="scss">
@import './scss/main.scss';

.results {
  margin-top: 50px;
}

.is-active {
  border: 3px solid #23d160
}
</style>
