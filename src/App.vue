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
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
html {
    box-sizing: border-box;
}

*,
*::before,
*::after {
    box-sizing: inherit;
}

html,
body {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
}

body {
    background: #ffffff url('assets/geometry2.png'); /* Background pattern from Subtle Patterns */
    font-family: 'Coda', cursive;
}

.container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

h1 {
    font-family: 'Open Sans', sans-serif;
    font-weight: 300;
}

/*
 * Styles for the deck of cards
 */

.deck {
    width: 660px;
    min-height: 680px;
    background: linear-gradient(160deg, #02ccba 0%, #aa7ecd 100%);
    padding: 32px;
    border-radius: 10px;
    box-shadow: 12px 15px 20px 0 rgba(46, 61, 73, 0.5);
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    margin: 0 0 3em;
}

.deck .card {
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

.deck .card.open {
    transform: rotateY(0);
    background: #02b3e4;
    cursor: default;
}

.deck .card.show {
    font-size: 33px;
}

.deck .card.match {
    cursor: default;
    background: #02ccba;
    font-size: 33px;
}

/*
 * Styles for the Score Panel
 */

.score-panel {
    text-align: left;
    width: 345px;
    margin-bottom: 10px;
}

.score-panel .stars {
    margin: 0;
    padding: 0;
    display: inline-block;
    margin: 0 5px 0 0;
}

.score-panel .stars li {
    list-style: none;
    display: inline-block;
}

.score-panel .restart {
    float: right;
    cursor: pointer;
}

/**
 *  Styles for Modal Message
 */

.modal-background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.438);
}

.modal-body {
    position: relative;
    width: 400px;
    top: 50%;
    left: 50%;
    background: #fff;
    transform: translate(-50%, -50%);
    display: flex;
}

.modal-body_container {
    display: flex;
    width: 100%;
    justify-content: center;
    align-content: center;
    flex-direction: column;
}

.modal-heading {
    display: flex;
    justify-content: center;
    align-content: flex-start;
    padding: 0 1.5rem;
    border-bottom: 1px solid gray;
    width: 100%;
    order: 1;
}

.modal-title {
    display: flex;
    justify-content: flex-start;
}

.modal-main {
    display: flex;
    justify-content: center;
    align-content: center;
    width: 100%;
    order: 2;
    flex-direction: column;
    border-bottom: 1px solid gray;
}

.modal-main_title {
    display: flex;
    width: 100%;
    justify-content: center;
}

.modal-main_paragraph {
    display: flex;
    width: 100%;
    justify-content: center;
    padding: 0 10px;
    text-align: center;
}

.modal-buttons {
    display: flex;
    order: 3;
    justify-content: space-between;
    align-content: center;
    padding: 30px 50px;
}

.quit {
    display: flex;
    text-align: center;
    padding: 10px;
}

.play-again {
    display: flex;
    text-align: center;
    padding: 10px;
}

.hide {
    display: none;
}
</style>
