<template>
  <li class="card"><i class="fa fa-anchor"></i></li>
</template>

<script>
export default {
  name: 'Card',
  // props: {
  //   card: Array
  // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
    height: 125px;
    width: 125px;
    background: #2e3d49;
    font-size: 0;
    color: #ffffff;
    border-radius: 8px;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    box-shadow: 5px 2px 20px 0 rgba(46, 61, 73, 0.5);
}

.card.open {
    transform: rotateY(0);
    background: #02b3e4;
    cursor: default;
}

.card.show {
    font-size: 33px;
}

.card.match {
    cursor: default;
    background: #02ccba;
    font-size: 33px;
}
</style>
