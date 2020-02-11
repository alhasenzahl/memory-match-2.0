<template>
  <div>
    <div class="container">
      <header>
          <h1>Matching Game</h1>
      </header>

      <section class="score-panel">
        <ul class="stars">
          <li class="star-icon"><i class="fa fa-star"></i></li>
          <li class="star-icon"><i class="fa fa-star"></i></li>
          <li class="star-icon"><i class="fa fa-star"></i></li>
        </ul>
        <span class="moves">0</span> Moves

          <div class="restart">
          <i class="fa fa-repeat"></i>
        </div>
      </section>
      <section>
          <span id="stop-watch">00:00</span>
      </section>
      <ul class="deck"></ul>
    </div>
    <div class="modal-background hide">
      <div class="modal-body">
          <div class="modal-body_container">
              <div class="modal-heading">
                  <h2 class="modal-title">Game Stats</h2>
              </div>
              <div class="modal-main">
                  <h3 class="modal-main_title"></h3>
                  <p class="modal-main_paragraph"></p>
              </div>
              <div class="modal-buttons">
                  <button class="quit-game">Quit</button>
                  <button class="play-again">Play Again</button>
              </div>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
