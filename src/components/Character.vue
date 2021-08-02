<template>
  <section class="outer-box">
    <span class="character-name">
      {{ character.name }}
    </span>

    <div class="health-border">
      <div class="health" :style="{'width':character.health + '%'}"></div>
    </div>
  </section>
</template>

<script>
import { ref } from "vue";

export default {
  props: ["character"],
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
  background: rgb(170, 170, 170);
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
}
</style>