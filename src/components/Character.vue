<template>
  <section class="outer-box">
    <span class="character-name">
      {{ character.name }}
    </span>

    <div class="health-border">
      <div class="health" :style="{ width: characterHealth + '%' }">
        {{ Math.round(characterHealth) }}
      </div>
    </div>
  </section>
</template>

<script>
import { ref } from "vue";

export default {
  props: ["character"],
  computed: {
    characterHealth() {
      return this.character.health;
    },
  },
};
</script>

<style>
section {
  width: 90%;
  max-width: 40rem;
  margin: auto;
}
.outer-box {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  background: rgba(245, 245, 245, 0.486);
  border-radius: 12px;
}
.character-name {
  font-weight: bold;
}

.health-border {
  width: 90%;
  border: solid 2px black;
  background-color: seashell;
  border-radius: 3px;
  margin: 5px auto;
}
.health {
  width: 100%;
  height: 20px;
  background: rgb(63, 2, 2);
  color: white;
}
</style>