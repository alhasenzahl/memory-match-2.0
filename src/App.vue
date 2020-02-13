<template>
    <div>
        <div class="container">
            <header>
                <h1>Matching Game</h1>
            </header>

            <section class="score-panel">
                <ul class="stars">
                    <li
                        class="star-icon"
                        :key="star.key"
                        v-for="star in stars"
                    >
                        <i :class="star.symbol" v-if="star.isShown"></i>
                    </li>
                </ul>
                <span class="moves">{{ clickCounter }}</span> Moves

                <div class="restart">
                    <i class="fa fa-repeat" @click="restartGame()"></i>
                </div>
            </section>
            <section>
                <span id="stop-watch">00:00</span>
            </section>
            <ul class="deck">
                <li 
                    class="card" 
                    :class="{
                        'open': card.isOpen,
                        'show': card.isShown,
                        'match': card.isMatched
                    }" 
                    :key="card.key" 
                    v-for="card in cards" 
                    @click="flipCards(card)"
                >
                    <i :class="card.symbol"></i>
                </li>
            </ul>
        </div>
        <div class="modal-background" v-if="showModal">
            <div class="modal-body">
                <div class="modal-body_container">
                    <div class="modal-heading">
                        <h2 class="modal-title">Game Stats</h2>
                    </div>
                    <div class="modal-main">
                        <h3 class="modal-main_title">Title</h3>
                        <p class="modal-main_paragraph">Message</p>
                    </div>
                    <div class="modal-buttons">
                        <button class="quit-game" @click="toggleModal()">Quit</button>
                        <button class="play-again" @click="restartGame()">Play Again</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
// import Star from './components/Star.vue'
// import Card from './components/Card.vue'
// import Modal from './components/Modal.vue'

export default {
    name: 'App',
    // components: {
    //     Star
    //     // Modal,
    //     // Card
    // },
    data: () => {
        return {
            cards: [
                { symbol: 'fa fa-anchor', key: 1, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-anchor', key: 2, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-bomb', key: 3, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-bomb', key: 4, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-bicycle', key: 5, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-bicycle', key: 6, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-diamond', key: 7, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-diamond', key: 8, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-paper-plane-o', key: 9, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-paper-plane-o', key: 10, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-cube', key: 11, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-cube', key: 12, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-leaf', key: 13, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-leaf', key: 14, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-bolt', key: 15, isOpen: false, isShown: false, isMatched: false },
                { symbol: 'fa fa-bolt', key: 16, isOpen: false, isShown: false, isMatched: false }
            ],
            stars: [
                { symbol: 'fa fa-star', key: 1, isShown: true },
                { symbol: 'fa fa-star', key: 2, isShown: true },
                { symbol: 'fa fa-star', key: 3, isShown: true },
            ],
            openCards: [],
            matchedCards: [],
            clickCounter: 0,
            showModal: false
        }
    },
    created() {
        this.shuffle(this.cards);
    },
    methods: {
        shuffle(array) {
            let currentIndex = array.length, temporaryValue, randomIndex;

            while (currentIndex !== 0) {
                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex -= 1;
                temporaryValue = array[currentIndex];
                array[currentIndex] = array[randomIndex];
                array[randomIndex] = temporaryValue;
            }

            return array;
        },
        restartGame() {
            if (this.showModal) {
                this.showModal = ! this.showModal;
            }

            this.openCards.forEach((card) => {
                card.isOpen = ! card.isOpen;
                card.isShown = ! card.isShown;
            });

            this.matchedCards.forEach((card) => {
                card.isMatched = ! card.isMatched;
                card.isShown = ! card.isShown;
            });

            // call resetStars() function
            this.clickCounter = 0;
            this.matchedCards = [];
            this.openCards = [];
            // timer goes back to 0
            // refresh page function call??
        },
        matchCards() {
            if (this.openCards.length === 2) {
                setTimeout(() => {
                    let cardOne = this.openCards[0];
                    let cardTwo = this.openCards[1];

                    if (cardOne.symbol === cardTwo.symbol) {
                        cardOne.isOpen = ! cardOne.isOpen;
                        cardTwo.isOpen = ! cardTwo.isOpen;
                        cardOne.isMatched = ! cardOne.isMatched;
                        cardTwo.isMatched = ! cardTwo.isMatched;

                        this.matchedCards.push(cardOne, cardTwo);
                        this.wonGame();
                    } else {
                        cardOne.isOpen = ! cardOne.isOpen;
                        cardTwo.isOpen = ! cardTwo.isOpen;
                        cardOne.isShown = ! cardOne.isShown;
                        cardTwo.isShown = ! cardTwo.isShown;
                    }

                    this.openCards = [];
                }, 1000);
            }
        },
        wonGame() {
            if (this.matchedCards.length === 16) {
                setTimeout(() => {
                    this.showModal = ! this.showModal;
                }, 1000);
            }
                // call stopTimer() function
                // add a 2.5 sec setTimeout to the rest of this
                    // call showModal() function
                    // add click listeners to play again and quit buttons on modal
        },
        flipCards(card) {
            if (! card.isOpen && ! card.isShown && ! card.isMatched) {
                // Start timer
                this.openCards.push(card);

                if (this.openCards.length <= 2) {
                    card.isOpen = ! card.isOpen;
                    card.isShown = ! card.isShown;

                    this.clickCounter++;
                    
                    this.matchCards();
                    // call starCount() function
                }
            }
        },
        toggleModal() {
            this.showModal = ! this.showModal;
            // Change style to display: block to show modal
            // create variable for time element.innerHTML

            // IF starCount is 3
                // set message text for this score
            // IF starCount is 2
                // set message text for this score
            // IF starCount is 1
                // set message text for this score
        },
    }
}
</script>

<style>
#app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}
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

.score-panel .restart {
    float: right;
    cursor: pointer;
}

.star-icon {
    list-style: none;
    display: inline-block;
}




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
