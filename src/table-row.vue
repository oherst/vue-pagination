<template>
  <div class="row">
    {{ item.title }}
  </div>
</template>

<script>
export default {
  props: ['item']
}
</script>

<style lang="scss">
.row {
  //border: 1px solid #eee;
  //padding: 7px;
}
</style>
