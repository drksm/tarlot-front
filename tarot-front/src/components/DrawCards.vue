<template>
  <div class="container">
    <button v-if="!isShuffled" @click="shuffle">洗牌</button>
    <div class="cards">
      <transition-group name="card" tag="div">
        <div
        v-if="isShuffled"
        class="card"
        v-for="card in cards"
        :key="card.id"
        @click="chooseCard(card)"
        :style="{ top: `${card.top}%`, left: `${card.left}%`, zIndex: card.zIndex, transform: `rotate(${card.rotate}deg)`, animationDelay: `${card.id * 0.1}s`, opacity: card.opacity, transition: card.transition }"
      >
          <div class="card-face" :class="{ flipped: card.flipped }">
            <div class="card-back">
            </div>
            <div class="card-front">
              <img class="card-content" :src="card.image" alt="">
            </div>
          </div>
        </div>
      </transition-group>
    </div>
    <div v-if="chosenCards.length === 3" class="modal">
      <div class="modal-content">
        <h3>您选择的牌是：</h3>
        <p v-for="card in chosenCards" :key="card.id"><img :src="card.image" alt=""></p>
        <button @click="redo">再抽一次</button>
        <button @click="listenAI">倾听AI</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShuffled: false,
      cards: Array.from({ length: 78 }, (_, i) => ({
        id: i,
        image: `/img/${i+1}.png`,
        flipped: false,
        ...this.getCardPosition(i)
      })),
      chosenCards: [],
    };
  },
  methods: {
    getCardPosition(id) {
      let top, left, zIndex, rotate;
      const radius = 10;
      const circleCenter = { x: 50, y: 50 }; // The center of the circle
      if (id < 24) {
        top = 15+(id / 24) * 35;
        left = 40;
        zIndex = id;
        rotate = 90;
      } else if (id < 54) {
        const theta = (1-(id - 24) / 30) * Math.PI; // Map card index to angle in radians
        // Calculate the card position using the center of the circle
        top = circleCenter.y + radius * Math.sin(theta);
        left = circleCenter.x + radius * Math.cos(theta);
        zIndex =  id;
        // The angle needs to be adjusted by -90 degrees to get the correct orientation
        rotate = theta * (180 / Math.PI) - 90;
      } else {
        top = 23+ ((77 - id) / 24) * 28;
        left = 60;
        zIndex =  id;
        rotate = 270;
      }
  return { top, left, zIndex, rotate };
},

shuffle() {
      this.isShuffled = true;
      this.cards = this.cards
        .sort((a, b) => a.id - b.id) // ensure they're sorted by ID first
        .map((card, i) => {
          const delay = i * 0.1; // increase delay for each card
          return { 
            ...card, 
            ...this.getCardPosition(i), 
            flipped: false, 
            opacity: 0, 
            transition: `opacity 0.15s ${delay}s ease-in-out` 
          };
        });
      setTimeout(() => { // after some time, make all cards visible
        this.cards.forEach(card => card.opacity = 1);
      }, 100);
      this.chosenCards = [];
    },
    chooseCard(card) {
      if (this.chosenCards.length >= 3 || card.flipped) return;
      this.chosenCards.push(card);
      card.flipped = true;
    },
    redo() {
      this.isShuffled = false;
      this.shuffle();
    },
    listenAI() {
      alert('听取AI的建议');
    },
  },
};
</script>

<style scoped>
.container {
  perspective: 1000px;
}

.cards {
  position: relative;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  overflow: auto;
}

.card {
  position: absolute;
  width: 60px;
  height: 90px;
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  text-align: center;
  transform-style: preserve-3d;
  cursor: pointer;
  transition: transform 1s;
}

.card-face.flipped {
  transform: rotateY(180deg);
}

.card-back,
.card-front {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid black;
}

.card-back {
  background-color: white;
  transform: rotate(0deg);
}

.card-front {
  background-color: gray;
  transform: rotateY(180deg) rotate(0deg);
}

.card-front img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.modal {
  display: block;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0,0,0,0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}
</style>