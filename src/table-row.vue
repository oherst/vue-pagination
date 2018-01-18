<template>
  <div class="row">
    {{ item.name }}
  </div>
</template>

<script>
export default {
  props: ['item']
}
</script>

<style lang="scss">
.row {
  display: flex;
  align-items: center;
  width: 100%;
  padding: 0 10px;
}
</style>
