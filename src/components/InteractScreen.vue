<template>
    <div class="screen" :data-grid="gridSize">
        <card-flip 
        v-for="(card, index ) in cardcontext" 
        :key="index" 
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{index: index, value: card}"
        @onFlip="checkRule($event)"
        />
    </div>
</template>

<script>
import CardFlip from './Card.vue'

export default {
    data() {
        return {
            rules: [], 
            matchedPairs: 0, 
            

        }
    },

    computed: {
        totalPairs() {
            return this.cardcontext.length /2;
        },
        isGameOver() {
            return this.matchedPairs === this.totalPairs;
        },
        gridSize() {
            const total = this.cardcontext.length;
            if (total === 16) return "4x4";
            if (total === 36) return "6x6";
            if (total === 64) return "8x8";
            if (total === 100) return "10x10";
            return "4x4";
        }
    },

    components: {
        CardFlip,
    },
    
    props: {
    cardcontext: {
      type: Array,
      default: () => [],
        }
    },
    methods: {
        checkRule(card) {
            console.log("Card flipped:", card);

            if(this.rules.length == 2) 
                return false;

            this.rules.push(card);
            // Vô hiệu hóa sự kiện click cho card vừa được mở
            this.$refs[`card-${card.index}`][0].$el.style.pointerEvents = "none";
            console.log(this.rules)
            
            if(this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
                console.log("Matched");
                this.matchedPairs++;
                // Disable onclick event for these cards
                this.$refs[`card-${this.rules[0].index}`][0].$el.style.pointerEvents = "none";
                this.$refs[`card-${this.rules[1].index}`][0].$el.style.pointerEvents = "none";
                
                // Reset rules ngay lập tức cho matched case
                this.rules = [];
                
                //check if game is over
                if(this.isGameOver) {
                    console.log("Game Over");
                    setTimeout(() => {
                        this.$emit("onGameOver", {
                            matchedPairs: this.matchedPairs,
                            totalPairs: this.totalPairs
                        });
                    }, 500);
                }
            }
            else if(this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
                console.log("Not matched");
                
                // ✅ Lưu references TRƯỚC khi reset để tránh race condition
                const card1Index = this.rules[0].index;
                const card2Index = this.rules[1].index;
                
                // Reset rules ngay lập tức
                this.rules = [];
                
                // Flip back the cards sau 800ms - dùng saved references
                setTimeout(() => {
                    this.$refs[`card-${card1Index}`][0].onFlipBack();
                    this.$refs[`card-${card2Index}`][0].onFlipBack();
                    
                    // Kích hoạt lại sự kiện click cho cả 2 cards
                    this.$refs[`card-${card1Index}`][0].$el.style.pointerEvents = "auto";
                    this.$refs[`card-${card2Index}`][0].$el.style.pointerEvents = "auto";
                }, 800);
            }
            else return false;
        }   
    }
    
}
</script>

<style scoped>
.screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: var(--dark);
    color: var(--light);
    display: grid;
    gap: 0.5rem;
    padding: 1rem;
    box-sizing: border-box;
    justify-content: center;
    align-content: center;
    overflow: hidden;
}

/* Grid layouts cho từng difficulty */
.screen[data-grid="4x4"] {
    grid-template-columns: repeat(4, 90px);
    grid-template-rows: repeat(4, 120px);
    gap: 1rem;
}

.screen[data-grid="6x6"] {
    grid-template-columns: repeat(6, 80px);
    grid-template-rows: repeat(6, 100px);
    gap: 0.8rem;
}

.screen[data-grid="8x8"] {
    grid-template-columns: repeat(8, 70px);
    grid-template-rows: repeat(8, 90px);
    gap: 0.6rem;
}

.screen[data-grid="10x10"] {
    grid-template-columns: repeat(10, 60px);
    grid-template-rows: repeat(10, 75px);
    gap: 0.4rem;
    padding: 0.5rem;
}
</style>