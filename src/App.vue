<template >
  <template v-if="!gameOver">
    <Character :character="monster" />
    <Character :character="player" />
    <button @click="playerAttack">Basic Attack</button> 
    <!-- <button @click="specialAttack">Special Attack</button> 
    <button @click="heal">Self Heal</button> 
    <button @click="dodge">Dodge</button>  -->
  </template>
  <template v-else>
    <h1>Game Over</h1>
    <button @click="player.health = 100; monster.health = 100">
      Restart The Game
    </button>
  </template>
</template>



<script>
import Character from "./components/Character.vue";
import { ref, watch } from "vue";
export default {
  components: { Character },

  setup() {
    const player = ref({ name: "Human", health: 100 });
    const monster = ref({ name: "Monster", health: 100 });
    const gameOver = ref(false);
    const attack = (character, max, min) => {
      const hitPoint = Math.random() * (max - min) + min;

      if (character.value.health - hitPoint > 0)
        character.value.health -= hitPoint;
      else character.value.health = 0;
    };

    const playerAttack = () => {
      attack(monster, 25, 10);
      attack(player, 35, 10);
    };

    watch(player.value, (newValue, oldValue) => {
      if (player.value.health == 0) {
        console.log("player h: " + player.value.health);
        gameOver.value = true;
      } else {
        gameOver.value = false;
      }
    });

    return {
      playerAttack,
      player,
      monster,
      gameOver,
    };
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  background: grey;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  margin-top: 60px;
}
</style>
