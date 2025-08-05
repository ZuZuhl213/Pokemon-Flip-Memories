<template>
    <div class="card"
        :class="{'disabled': isDisabled}">
        <div class="card-inner" 
        :class="{'is-flipped' : isFlipped }" 
        @click="onToggleFlipCard">
            <div class="card-face card-face-front">
                <div class="card-content"></div>
            </div>
            <div class="card-face card-face-back">
                <div 
                class="card-content" 
                :style="{backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})` }">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CardFlip',

    props: {
        imgBackFaceUrl: {
            type: String,
            required: true,
        },
        card: {
            type: [String, Number, Object, Array]
        },
    },

    data() {
        return {
            isFlipped: false,
            isDisabled: false,
        }
    },

    methods: {
        onToggleFlipCard() {
            if(this.isDisabled) return false;
            this.isFlipped = !this.isFlipped;
            if(this.isFlipped) this.$emit("onFlip", this.card);       
        },
        onFlipBack() {
            this.isFlipped = false;
        }
    }
}
</script>

<style lang="css" scoped>
.card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
    width: 90px;
    height: 120px;;
}

.card-inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    position: relative;

}

.card.disabbled .card-inner {
    cursor: default;

}

.card-inner.is-flipped {
    transform: rotateY(-180deg);

}

.card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1px;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card-face-back {
    transform: rotateY(180deg);
    background-color: var(--light);

}

.card-face-front {
    /* background-color: var(--light);
    transform: rotateY(-180deg); */
}

.card-face-front .card-content {
    background: url('/home/hoang/pokemon-flip-memories/src/assets/images/icon_back.png') no-repeat center center;
    width: 100%;
    height: 100%;
    background-size: 40px 40px;
}

.card-face-back .card-content {
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;        
}
</style>