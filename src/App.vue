<template >
  
    <h1 style="background-color:whitesmoke; padding:20px">The Monster Slayer</h1>
  

  <div v-if="gameOver || youWon">
    <h1>{{ gameResult }}</h1>
    <button @click="restart">Restart The Game</button>
  </div>

  <div class="platform" v-else>
    <div class="game">
      <Character :character="monster" />
      <Character :character="player" />
      <div class="buttons">
        <button @click="playerAttack">Basic Attack</button>
        <button @click="specialAttack" :disabled="saCounter < 4 ? true : false">
          Special Attack
        </button>
        <button @click="heal" :disabled="hlCounter < 3 ? true : false">
          Self Heal
        </button>
        <!-- 
    <button @click="dodge">Dodge</button>  -->
      </div>
    </div>
    <ul class="indicators">
      <li>
        {{ 4 - saCounter > 0 ? 4 - saCounter : 0 }} - Special Attack Delay
      </li>
      <li>{{ 3 - hlCounter > 0 ? 3 - hlCounter : 0 }} - Heal Delay</li>
    </ul>
  </div>
</template>



<script>
import Character from "./components/Character.vue";
import { ref, watch, watchEffect } from "vue";
export default {
  components: { Character },

  setup() {
    const player = ref({ name: "Player", health: 100 });
    const monster = ref({ name: "Monster", health: 100 });
    const gameOver = ref(false);
    const youWon = ref(false);
    let gameResult = ref("");
    let saCounter = ref(4);
    let hlCounter = ref(3);
    const attack = (character, max, min) => {
      const hitPoint = Math.random() * (max - min) + min;

      if (character.value.health - hitPoint > 0)
        character.value.health -= hitPoint;
      else character.value.health = 0;
    };

    const playerAttack = () => {
      attack(monster, 13, 5);
      setTimeout(() => {
        attack(player, 20, 7);
      }, 500);

      saCounter.value++;
      hlCounter.value++;
    };

    const specialAttack = () => {
      if (saCounter.value > 3) {
        attack(monster, 20, 9);
        setTimeout(() => {
          attack(player, 13, 5);
        }, 500);

        saCounter.value = 1;
        hlCounter.value++;
      }
    };

    const heal = () => {
      if (hlCounter.value > 2) {
        const healAmount = Math.random() * (20 - 11) + 11;
        if (player.value.health + healAmount <= 100)
          player.value.health += healAmount;
        else player.value.health = 100;

        setTimeout(() => {
          attack(player, 20, 7);
        }, 500);

        hlCounter.value = 1;
        saCounter.value++;
      }
    };

    watchEffect(() => {
      if (monster.value.health == 0) {
        youWon.value = true;
        gameResult.value = "You Won!";
      } else {
        youWon.value = false;
      }

      if (player.value.health == 0 && !youWon.value) {
        gameOver.value = true;
        gameResult.value = "You Lose! Game Over";
      } else {
        gameOver.value = false;
      }
    });

    function restart() {
      player.value.health = 100;
      monster.value.health = 100;
      saCounter.value = 4;
      hlCounter.value = 3;
    }

    return {
      playerAttack,
      specialAttack,
      saCounter,
      hlCounter,
      heal,
      player,
      monster,
      gameOver,
      youWon,
      gameResult,
      restart,
    };
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  background: rgba(238, 94, 69, 0.959);
  /* background: rgba(238, 136, 69, 0.959) */
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  /* height: 60vh; */
  margin: auto;
}
.platform {
  margin-top: 20vh;
  display: flex;
  justify-content: center;
  /* align-items: center; */
}
.game {
  width: 40rem;
}

.buttons {
  display: flex;
  justify-content: center;
}
button {
  padding: 1rem;
  margin: 1rem;
  font-weight: bold;
  background-color: whitesmoke;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.indicators {
  text-align: left;
  list-style: none;
  font-weight: bold;
  color: rgb(0, 0, 0);
}
</style>
