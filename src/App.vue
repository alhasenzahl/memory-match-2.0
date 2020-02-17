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
                        <h2 class="modal-title">You Won!</h2>
                    </div>
                    <div class="modal-main">
                        <p class="modal-main_title">{{ message }}</p>
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
                { symbol: 'fa fa-star', key: 3, isShown: true }
            ],
            modalMessage: [
                { message: 'What a performance! Nice work, Superstar!' },
                { message: 'Solid performance, keep up the good work!' },
                { message: 'No worries, there is always next time!' }
            ],
            openCards: [],
            matchedCards: [],
            clickCounter: 0,
            showModal: false,
            message: '',
            firstClick: false
            // Add a firstClick property here
            // Add data properties for seconds and minutes
        }
    },
    created() {
        this.shuffle(this.cards);
    },
    updated() {
        // On first card click, call function to initiate timer using setInterval()
        this.initTimer();
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

            this.resetStars();
            this.clickCounter = 0;
            this.matchedCards = [];
            this.openCards = [];
            // timer goes back to 0
        },
        matchCards() {
            if (this.openCards.length === 2) {
                this.updateScoring();
                
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
            // call stopTimer() function
            if (this.matchedCards.length === 16) {
                setTimeout(() => {
                    this.toggleModal();
                }, 1000);
            }
        },
        flipCards(card) {
            if (! card.isOpen && ! card.isShown && ! card.isMatched) {
                // Start timer
                this.openCards.push(card);

                if (this.openCards.length <= 2) {
                    card.isOpen = ! card.isOpen;
                    card.isShown = ! card.isShown;
                    
                    this.matchCards();
                }
            }
        },
        toggleModal() {
            this.showModal = ! this.showModal;

            if (this.stars[0].isShown) {
                this.message = this.modalMessage[0].message;
            } else if (this.stars[1].isShown) {
               this.message = this.modalMessage[1].message;
            } else {
                this.message = this.modalMessage[2].message;
            }
        },
        updateScoring() {
            this.clickCounter++;

            if (this.clickCounter > 10 && this.stars[0].isShown) {
                this.stars[0].isShown = ! this.stars[0].isShown;
            } else if (this.clickCounter > 20 && this.stars[1].isShown) {
                this.stars[1].isShown = ! this.stars[1].isShown;
            }
        },
        resetStars() {
            if (! this.stars[0].isShown) {
                this.stars[0].isShown = ! this.stars[0].isShown;
            }

            if (! this.stars[1].isShown) {
                this.stars[1].isShown = ! this.stars[1].isShown;
            }
        },
        initTimer() {
            if (! this.firstClick) {
                // this.startTimer();
                this.firstClick = ! this.firstClick;
                console.log('clicked card');
            }
        },
        startTimer() {
            // create timer instance using setInterval()
        },
        updateTimer() {
            // use seconds and minutes data properties here to update the clock time
        },
        stopTimer() {
            // stop clock, call it when all of the matches have been found
        }
        //Function that creates the timer for the game
        // function gameTimer() {
        //     timerId = setInterval(function() {
        //         time += 1;
        //         displayTimer();
        //     }, 1000);
        // }

        //Function that connects/displays the timer to the DOM
        // function displayTimer() {
        //     const timer = document.getElementById('stop-watch');
        //     const minutes = Math.floor(time / 60);
        //     const seconds = time % 60;

        //     if (seconds < 10) {
        //         timer.innerHTML = `${minutes}:0${seconds}`;
        //     } else {
        //         timer.innerHTML = `${minutes}:${seconds}`;
        //     }
        // }

        //Function that stops the clock when the game is over
        // function stopTimer() {
        //     clearInterval(timerId);
        // }
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
    border-radius: 10px;
    min-height: 400px;
}

.modal-body_container {
    width: 100%;
}

.modal-heading {
    padding: 30px 0 30px;
}

.modal-title {
    text-align: center;
}

.modal-main {
    padding: 50px 0;
    margin: auto;
    display: block;
    border-top: 1px solid grey;
    border-bottom: 1px solid grey;
}

.modal-main_title {
    display: flex;
    width: 100%;
    justify-content: center;
    padding: 0 20px;
}

.modal-buttons {
    display: block;
    margin: auto;
    padding-top: 50px;
    width: 200px;
}

.quit-game {
    width: 75px;
    margin-right: 50px;
    padding: 10px 0;
    border-radius: 8px;
}

.play-again {
    width: 75px;
    padding: 10px 0;
    border-radius: 8px;
}

.hide {
    display: none;
}




/** CURRENT TO DO LIST:
    - ELIMINATE THE ABILITY TO SCROLL WHEN THE MODAL IS OPEN
    - FINISH IMPLEMENTING THE TIMER
    - RESTYLE MODAL
    - IMPLEMENT HIAC IMAGES AND GAME DESIGN
    - ONCE ALL FUNCTIONALITY WORKS RIGHT, MAKE IT MORE EFFICIENT!
 */

</style>
