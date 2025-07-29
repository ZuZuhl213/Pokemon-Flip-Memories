<template>
    <div class="screen">
        <h1>Pokémon Memmories</h1>
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