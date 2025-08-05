<template>
    <div class="screen">
        <h1>Pokémon Memories</h1>
        <h3>Select mode to start</h3>
        <div class="action">
            <button @click = "onStart(16)"> 
                <span>4x4</span>
                <span>Easy</span>
            </button>
            <button @click = "onStart(36)">
                <span>6x6</span>
                <span>Normal</span>
            </button>
            <button @click="onStart(64)">
                <span>8x8</span>
                <span>Hard</span>
            </button>
            <button @click="onStart(100)">
                <span>10x10</span>
                <span>Super Hard</span>
            </button>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        onStart(totalOfBlock) {
            this.$emit("onStart", { totalOfBlock})
        }
    }
}
</script>

<style scoped>
.screen {
    width: 100%;
    height: 100vh;
    background-color: var(--dark);
    color: var(--light);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 2rem;
    box-sizing: border-box;
}

h1 {
    font-size: 3rem;
    font-weight: bold;
    margin-bottom: 1rem;
    text-transform: uppercase;
    letter-spacing: 2px;
}

h3 {
    font-size: 1.2rem;
    margin-bottom: 3rem;
    opacity: 0.8;
}

.action {
    display: flex;
    gap: 2rem;
    flex-wrap: wrap;
    justify-content: center;
}

button {
    background: rgba(255, 255, 255, 0.1);
    border: 2px solid rgba(255, 255, 255, 0.3);
    border-radius: 1rem;
    color: var(--light);
    padding: 2rem 3rem;
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-width: 120px;
}

button:hover {
    background: rgba(255, 255, 255, 0.2);
    border-color: rgba(255, 255, 255, 0.5);
    transform: translateY(-5px);
}

button span:first-child {
    font-size: 2rem;
    font-weight: bold;
    margin-bottom: 0.5rem;
}

button span:last-child {
    font-size: 1rem;
    opacity: 0.8;
}
</style>