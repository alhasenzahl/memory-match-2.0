<template>
    <div>
        <div class="container">
            <header>
                <h1>Hell in a Cell Memory Match</h1>
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

                <button class="restart" @click="restartGame()">
                    <i class="fa fa-repeat"></i>
                </button>
            </section>
            <section class="timer-wrap">
                <span id="stop-watch">{{ formattedTime }}</span>
            </section>
            <ul class="deck">
                <li 
                    class="card" 
                    :key="card.key"
                    v-for="card in cards" 
                >
                    <button class="card-button" @click="flipCards(card)">
                        <img
                            :src="card.front"
                            alt=""
                            class="card-front"
                            v-if="!card.isOpen && !card.isShown && !card.isMatched"
                            :class="{
                                'open': card.isOpen,
                                'show': card.isShown,
                                'match': card.isMatched
                            }" 
                        />
                        <img
                            :src="card.back"
                            alt=""
                            class="card-back"
                            v-if="card.isOpen || card.isShown || card.isMatched"
                            :class="{
                                'open': card.isOpen,
                                'show': card.isShown,
                                'match': card.isMatched
                            }" 
                        />
                    </button>
                </li>
            </ul>
        </div>
        <div class="modal-background" v-if="showModal">
            <div class="modal-body">
                <div class="modal-body_container" ref="modal" @keydown.tab="focusTrap">
                    <div class="modal-heading">
                        <h2 class="modal-title">You Won!</h2>
                    </div>
                    <div class="modal-main">
                        <p class="modal-main_title">{{ message }}</p>
                    </div>
                    <div class="modal-buttons">
                        <button class="quit-game js-modal-button" ref="modalButton" @click="toggleModal()">Quit</button>
                        <button class="play-again js-modal-button" @click="restartGame()">Play Again</button>
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
                { back: 'https://lh3.googleusercontent.com/9B6FPA8DEBi0iJRchbNEUQ8LrCzB3ZD2iS3Bo9GiHGkm4ob7uHF2DyzdxQjo8795LoazbJzJpzZDjZrydaOvtauNXDCiNQL76Ig9RrMRJ_OLq-AiTiJP4NqOPULavCiJabJxErkD=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 1, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/9B6FPA8DEBi0iJRchbNEUQ8LrCzB3ZD2iS3Bo9GiHGkm4ob7uHF2DyzdxQjo8795LoazbJzJpzZDjZrydaOvtauNXDCiNQL76Ig9RrMRJ_OLq-AiTiJP4NqOPULavCiJabJxErkD=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 2, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/FykfF6Xc-dXXiG4IR9cbQa-8aHQp0xZamiFrWkysa7GIWGgnArRIKV0bNvEZgtNkpL3QyVvMVhLLgr-4gH_EwI1ec0rH4jg9dP22BS2ns6mpAQKZgyTeKASFmD825AHMAaFHCxnY=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 3, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/FykfF6Xc-dXXiG4IR9cbQa-8aHQp0xZamiFrWkysa7GIWGgnArRIKV0bNvEZgtNkpL3QyVvMVhLLgr-4gH_EwI1ec0rH4jg9dP22BS2ns6mpAQKZgyTeKASFmD825AHMAaFHCxnY=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 4, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/NvmH0fDKfg2lZObnzUZGqAkIa5vploh0WPxqmW4_sEiIujFSXOlWMeOtMk0HZoIqzdlfxztI7shQszpFnBNFdprhaTfap9ATI6CJS-DjGyzqKSBB_ASt3KcfBIKV4OFn2tYixZ-N=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 5, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/NvmH0fDKfg2lZObnzUZGqAkIa5vploh0WPxqmW4_sEiIujFSXOlWMeOtMk0HZoIqzdlfxztI7shQszpFnBNFdprhaTfap9ATI6CJS-DjGyzqKSBB_ASt3KcfBIKV4OFn2tYixZ-N=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 6, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/19-6lBgJH6B62Rbk6sitLrYjsVvowH_o5xvN9uVe7BPMFRT6jVMxcZXQbZ0pJ_TVWZWk6w55q8Gs2nTXcrtN6rvhbjC2BE2Vp2Q_cxUZmfiVyCkc3XuBu8xvHGy8QQAqk7bAP9r9=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 7, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/19-6lBgJH6B62Rbk6sitLrYjsVvowH_o5xvN9uVe7BPMFRT6jVMxcZXQbZ0pJ_TVWZWk6w55q8Gs2nTXcrtN6rvhbjC2BE2Vp2Q_cxUZmfiVyCkc3XuBu8xvHGy8QQAqk7bAP9r9=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 8, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/6wwBs2ahf3fKQrH_xf1v7hztFG6tAB3qGidy16_azm4l4HU_oKFZpdBsnScvlnyK8qCETqO146Fa6W9dCn9W6pFU5n81K9O_LMLEsrFv-o2Ad4-426R9SmprG29yDiam5pI0pmiv=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 9, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/6wwBs2ahf3fKQrH_xf1v7hztFG6tAB3qGidy16_azm4l4HU_oKFZpdBsnScvlnyK8qCETqO146Fa6W9dCn9W6pFU5n81K9O_LMLEsrFv-o2Ad4-426R9SmprG29yDiam5pI0pmiv=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 10, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/JqNaOASQb7NFK16k1MWyTXQVpqVk35Q_VLGctG-BRgvRdJjnKmuC54k8lEbVEvPACXcyGXQkf1Dl3hpgR7KCru3BHESKSVek5d1JDohEKvtZDFcFPP1q999fEKj67Mwb4USM_P51=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 11, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/JqNaOASQb7NFK16k1MWyTXQVpqVk35Q_VLGctG-BRgvRdJjnKmuC54k8lEbVEvPACXcyGXQkf1Dl3hpgR7KCru3BHESKSVek5d1JDohEKvtZDFcFPP1q999fEKj67Mwb4USM_P51=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 12, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/nxz3dmc0yaly7cqmZ7IGTe6_K5QkgdXQCA4sKXMDv2Y5uOQdZps3rpXPtujUuvlG6N9TGGPiYfnvlsucyiATNsUqir-vJCUaQIMvMI_a90_PZLRR4YQGUbrJVHa3V0aVWDp_lv-0=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 13, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/nxz3dmc0yaly7cqmZ7IGTe6_K5QkgdXQCA4sKXMDv2Y5uOQdZps3rpXPtujUuvlG6N9TGGPiYfnvlsucyiATNsUqir-vJCUaQIMvMI_a90_PZLRR4YQGUbrJVHa3V0aVWDp_lv-0=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 14, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/lv5ie9DLFC8_YjELlTCMohS9Nk_BKZypYnMyfTNBnIk2zmphqKaLgAypjyIhIGQkbLKGiKXAQTqqfMShClec5KFUlfF7HE2CceLpGg39i6gpErlZSPaKu6v8tuM6UfDwrfizPgt3=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 15, isOpen: false, isShown: false, isMatched: false },
                { back: 'https://lh3.googleusercontent.com/lv5ie9DLFC8_YjELlTCMohS9Nk_BKZypYnMyfTNBnIk2zmphqKaLgAypjyIhIGQkbLKGiKXAQTqqfMShClec5KFUlfF7HE2CceLpGg39i6gpErlZSPaKu6v8tuM6UfDwrfizPgt3=w2400', front: 'https://is5-ssl.mzstatic.com/image/thumb/Video22/v4/9f/af/21/9faf21eb-755f-954d-9bee-fc45ece7e452/mzl.irbbcyrq.jpg/268x0w.jpg', key: 16, isOpen: false, isShown: false, isMatched: false }
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
            firstClick: false,
            interval: null,
            time: 0
        }
    },
    computed: {
        formattedTime() {
            const time = this.time;
            const minutes = Math.floor(time / 60);
            let seconds = time % 60;

            if (seconds < 10) {
                seconds = `0${seconds}`;
            }

            return `${minutes}:${seconds}`;
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
            if (this.matchedCards.length !== 16) {
                this.stopTimer();
            }

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
            this.time = 0;
            this.interval = null;
            this.firstClick = false;
        },
        matchCards() {
            if (this.openCards.length === 2) {
                this.updateScoring();
                
                setTimeout(() => {
                    let cardOne = this.openCards[0];
                    let cardTwo = this.openCards[1];

                    if (cardOne.back === cardTwo.back) {
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
                }, 1500);
            }
        },
        wonGame() {
            if (this.matchedCards.length === 16) {
                this.stopTimer();
                setTimeout(() => {
                    this.toggleModal();
                }, 1000);
            }
        },
        flipCards(card) {
            if (! this.firstClick) {
                this.startTimer();
                this.firstClick = ! this.firstClick;
            }

            if (! card.isOpen && ! card.isShown && ! card.isMatched) {
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

            this.modalFocus();
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
        startTimer() {
            this.interval = setInterval(() => {
                this.time += 1;
            },1000);
        },
        stopTimer() {
            clearInterval(this.interval);    
        },
        modalFocus() {
            this.nextTick().then(() => {
                console.log(this.$refs.modalButton);
            });
        },
        focusTrap(e) {
            const buttons = this.$refs.modal.querySelectorAll('.js-modal-button');
            const first = buttons[0];
            const last = buttons[buttons.length - 1];

            console.log(buttons);

            if (e.shiftKey) {
                if (document.activeElement === first) {
                    last.focus();
                    e.preventDefault();
                }
            } else if (document.activeElement === last) {
                first.focus();
                e.preventDefault();
            }
        }
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
    font-size: 10px;
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
    background-image: url('https://1.bp.blogspot.com/-qTwY85_O668/WAnKTDSTvUI/AAAAAAAAB2A/U3VL7wXsvJESu1CSJ304m7C8WQV7YZPzACLcB/s1600/1.JPG');
    background-size: cover;
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
    color: white;
    font-size: 3rem;
    padding: 0 2.7rem;
    text-align: center;
}

/*
 * Styles for the deck of cards
 */

.deck {
    max-width: 66rem;
    min-height: 68rem;
    width: calc(100% - 2rem);
    background: linear-gradient(160deg, #CC281D 0%, #FF3424 100%);
    padding: 2rem 0;
    border-radius: 1rem;
    box-shadow: 1.2rem 1.5rem 2rem 0 rgba(46, 61, 73, 0.5);
    display: grid;
    padding-inline-start: 0;
    grid-template-columns: 1fr 1fr;
    grid-gap: 2rem 0;
    margin: 0 0 3em;
}

.deck .card {
    max-height: 12.5rem;
    max-width: 12.5rem;
    background: #2e3d49;
    font-size: 0;
    color: #ffffff;
    border-radius: .8rem;
    cursor: pointer;
    justify-self: center;
    align-self: center;
    box-shadow: .5rem .2rem 2rem 0 rgba(46, 61, 73, 0.5);
}

.deck .card .card-back,
.deck .card .card-front {
    width: 100%;
    height: auto;
    border-radius: .8rem;
}

.deck .card .card-front {
    filter: grayscale(100%) contrast(70%);
}

.deck .card-button {
    background: none;
    border: none;
    padding: 0;
    cursor: pointer;
}





/*
 * Styles for the Score Panel
 */

.score-panel {
    text-align: left;
    width: 100%;
    max-width: 34.5rem;
    margin-bottom: 1rem;
    color: white;
    padding: 0 2rem;
    font-size: 1.5rem;
}

.score-panel .stars {
    margin: 0;
    padding: 0;
    display: inline-block;
    margin: 0 .5rem 0 0;
}

.score-panel .restart {
    float: right;
    cursor: pointer;
    color: white;
    background: none;
    border: none;
    font-size: 1.5rem;
}

.star-icon {
    list-style: none;
    display: inline-block;
    color: white;
}

#stop-watch {
    color: white;
    font-size: 1.5rem;
}

.timer-wrap {
    margin-bottom: 2rem;
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
    max-width: 40rem;
    width: calc(100% - 2rem);
    top: 50%;
    left: 50%;
    background: #fff;
    transform: translate(-50%, -50%);
    display: flex;
    border-radius: 1rem;
    min-height: 35rem;
}

.modal-body_container {
    width: 100%;
}

.modal-heading {
    padding: 0;
    font-size: 2.5rem;
}

.modal-title {
    text-align: center;
}

.modal-main {
    padding: 3rem 0;
    margin: auto;
    display: block;
    border-top: 1px solid grey;
    border-bottom: 1px solid grey;
    font-size: 1.6rem;
}

.modal-main_title {
    display: flex;
    width: 100%;
    justify-content: center;
    padding: 0 2rem;
}

.modal-buttons {
    display: block;
    margin: auto;
    padding-top: 5rem;
    width: 20rem;
}

.quit-game {
    width: 7.5rem;
    margin-right: 5rem;
    padding: 1rem 0;
    border-radius: .8rem;
    font-size: 1.2rem;
}

.play-again {
    width: 7.5rem;
    padding: 1rem 0;
    border-radius: .8rem;
    font-size: 1.2rem;
}

.hide {
    display: none;
}




@media only screen and (min-width: 480px) {
   .deck {
        grid-template-columns: 1fr 1fr 1fr;
    } 

    .score-panel {
        padding: 0;
    }
}

@media only screen and (min-width: 600px) {
   .deck {
        grid-template-columns: 1fr 1fr 1fr 1fr;
    } 
}
/** CURRENT TO DO LIST:
    - ELIMINATE THE ABILITY TO SCROLL WHEN THE MODAL IS OPEN
    - MAKE SURE BUTTONS IN MODAL ARE TABBABLE
    - ARE THERE WAYS TO MAKE THE GAME TABBABLE, MORE ACCESSIBLE OVERALL?
        - MAKE CARDS A BUTTON OR LINK
        - MAKE RESTART ICON A BUTTON
    - RESTYLE MODAL
        - ADD GAME INFORMATION TO WINNING MESSAGE?
    - BREAK OUT CODE INTO COMPONENTS?
    - ONCE ALL FUNCTIONALITY WORKS RIGHT, MAKE IT MORE EFFICIENT!
 */

</style>
