<template>
  <li class="star-icon"><i class="fa fa-star"></i></li>
</template>

<script>
export default {
  name: 'Star',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.star-icon {
    list-style: none;
    display: inline-block;
}
</style>