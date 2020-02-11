<template>
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
</template>

<script>
export default {
    name: 'Modal',
    props: {
        msg: String
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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