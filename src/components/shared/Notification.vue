<template lang="pug">
  .container
    .columns
      .column.is-5.is-offset-4
        .notification(:class="error ? 'is-danger': 'is-success'")
          slot(name="body") Algo salio mal
</template>

<script>
export default {
  props: {
    error: Boolean
  }
}
</script>

<style lang="scss" scoped>
  .notification {
    margin: 10px;
  }
</style>
