<template>
  <div v-if="showModal" class="modal">
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>
      <h2>Bienvenue sur Circle&nbsp;Master !</h2>
      <p>
        Le but du jeu est de maintenir le cercle plein entre les deux
        cercles cibles en maintenant et en relâchant le clic ou la pression sur l'écran.
      </p>
      <p>
        Un score est donné en fonction de la position du cercle plein. Essayez d'obtenir le meilleur score
        possible !
      </p>
      <button @click="closeModal">Jouer</button>
    </div>
  </div>
  <div v-if="showEndModal" class="modal">
    <div class="modal-content">
      <h2>Félicitations !</h2>
      <p>Votre score est : {{ userScore }}</p>
      <div>
        <button @click="shareScore">Partager mon score</button>
      </div>
      <div>
        <button @click="restartGame">Recommencer</button>
      </div>
    </div>
  </div>

  <p class="my-score">Mon score: {{ userScore }}</p>
  <div class="game-container">
    <div class="circle target-circle outer" :style="{
      width: outerCircleSize + 'px',
      height: outerCircleSize + 'px',
      borderRadius: outerCircleSize / 2 + 'px',
    }"></div>
    <div class="circle target-circle inner" :style="{
      width: (targetSize * innerCircleSizeRatio) + 'px',
      height: (targetSize * innerCircleSizeRatio) + 'px',
      borderRadius: (targetSize * innerCircleSizeRatio) / 2 + 'px',
    }"></div>
    <div class="circle main-circle" :style="{
      width: circleSize + 'px',
      height: circleSize + 'px',
      borderRadius: circleSize / 2 + 'px',
      backgroundColor: isBetweenTargetCircles ? 'green' : 'gray',
    }"></div>
  </div>
  <div class="click-area no-highlights" @mousedown="onPress" @touchstart="onPress" @mouseup="onRelease"
    @touchend="onRelease"></div>
</template>

<script lang="ts" setup>
import { classPrivateMethod } from '@babel/types';
import { ref, onMounted, onUnmounted, computed, watch } from 'vue';
import useClipboard from "vue-clipboard3";

const { toClipboard } = useClipboard();

const showModal = ref(true);

const closeModal = () => {
  showModal.value = false;
};

const showEndModal = ref(false);

const shareScore = async () => {
  const shareText = `⭕ J'ai obtenu ${userScore.value} points sur Circle Master ! ⭕\r\nRejoins-moi et bats mon score !\r\n\r\nhttps://circlemaster.nitocode.com`;
  try {
    await toClipboard(shareText);
    alert('Score copié dans le presse-papier ! Partagez-le avec vos amis.');
  } catch (e) {
    console.error(e);
    alert("Impossible de copier le texte. Partagez votre score manuellement.");
  }
};

const circleSizeSpeed = 3;
const outerCircleShrinkingSpeed = 0.5;
const outerCircleHeightRatio = 0.7;
const innerCircleSizeRatio = ref(0.9);
const innerAndMainCircleRatio = 0.65;
const maxSize = Math.round(window.innerHeight * outerCircleHeightRatio);
const targetSize = Math.round(maxSize * innerAndMainCircleRatio);
const minSize = Math.round(targetSize * innerCircleSizeRatio.value) - (maxSize - targetSize);
const circleSize = ref(minSize);
const isPressed = ref(false);
const userScore = ref(0);

const outerCircleSize = ref(maxSize);
let isShrinkingStarted = ref(false);

const isBetweenTargetCircles = computed(() => {
  const actualSize = circleSize.value;
  return (
    actualSize >= targetSize * innerCircleSizeRatio.value && actualSize <= outerCircleSize.value
  );
});

const updateCircleSize = () => {
  if (isPressed.value) {
    if (circleSize.value < maxSize) {
      circleSize.value += circleSizeSpeed;
    }
  } else {
    if (circleSize.value > minSize) {
      circleSize.value -= circleSizeSpeed;
    }
  }

  if (isBetweenTargetCircles.value) {
    userScore.value += 1;
  }

  if (isShrinkingStarted.value && outerCircleSize.value > targetSize * innerCircleSizeRatio.value) {
    outerCircleSize.value -= outerCircleShrinkingSpeed;
  } else if (isShrinkingStarted.value) {
    showEndModal.value = true;
  }
};

let interval: number | null = null;

const restartGame = () => {
  userScore.value = 0;
  circleSize.value = minSize;
  showEndModal.value = false;
  isShrinkingStarted.value = false;
  outerCircleSize.value = maxSize;
};

onMounted(() => {
  interval = setInterval(updateCircleSize, 20);
});

onUnmounted(() => {
  if (interval !== null) {
    clearInterval(interval);
  }
});

const onPress = () => {
  isPressed.value = true;
  isShrinkingStarted.value = true;
};

const onRelease = () => {
  isPressed.value = false;
};
</script>

<style scoped>
.game-container {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  position: relative;
}

.my-score {
  margin-top: 40px;
}

.circle {
  position: absolute;
  transition: all 0.2s ease;
}

.main-circle {
  z-index: 0;
}

.target-circle {
  border: 3px solid #f55;
  box-sizing: border-box;
  background-color: transparent;
}

.inner,
.outer {
  z-index: 1;
}

.click-area {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  cursor: pointer;
  z-index: 2;
}


.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  z-index: 1000;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
  max-width: 500px;
  text-align: center;
}

.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
  position: relative;
  z-index: 2;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}
</style>