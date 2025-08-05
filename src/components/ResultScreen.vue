<template>
    <div class="game-complete">
        <h1>🎉 Congratulations!</h1>
        <div class="stats">
            <p>Time: {{ results.playTimeSeconds }}</p>
        </div>
        <button @click="$emit('onPlayAgain')" class="restart-btn">
            Play Again
        </button>
        <button @click="$emit('onBackToMenu')" class="menu-btn">
            Main Menu
        </button>
    </div>
</template>

<script>
export default {
    name: 'ResultScreen',
    props: {
        results: {
            type: Object,
            required: true
        }
    },
    methods: {
        
    }
}
</script>

<style scoped>
.game-complete {
    text-align: center;
    padding: 2rem;
}
.stats {
    margin: 2rem 0;
}
.restart-btn, .menu-btn {
    background: #42b883;
    color: white;
    border: none;
    padding: 1rem 2rem;
    border-radius: 0.5rem;
    cursor: pointer;
    font-size: 1.1rem;
    margin: 0 0.5rem;
}
.menu-btn {
    background: #95a5a6;
}

h1 {
    font-size: 3rem;
    font-weight: bold;
    margin-bottom: 1rem;
    text-transform: uppercase;
    letter-spacing: 2px;
}
</style>