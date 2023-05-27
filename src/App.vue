<template>
  <h1>Color Test</h1>
  <h2>Which one is different?</h2>

  <div class="row-selection">
    <span class="row-text">Rows: </span>
    <input
      type="number"
      min="2"
      max="12"
      v-model.number="boardWidth"
      @change="newColor"
      class="row-selector"
    />
  </div>
  <div class="row-selection">
    <span class="row-text">Diff Rate: </span>
    <input
      type="number"
      min="0"
      max="255"
      v-model.number="diffRate"
      @change="newColor"
      class="row-selector"
    />
  </div>

  <div class="message">
    <span :class="{ correct: isCorrect, wrong: !isCorrect }">{{ msg }}</span>
  </div>

  <div class="board">
    <div class="row" v-for="n in boardWidth" :key="n">
      <span v-for="m in boardWidth" :key="m">
        <span v-if="(n - 1) * boardWidth + m === randomIdx"
          ><Block
            :color="diffColor"
            isDiff="true"
            @correct="handleCorrect"
          ></Block
        ></span>
        <span v-else
          ><Block
            :color="currentColor"
            isDiff="false"
            @wrong="handleWrong"
          ></Block
        ></span>
      </span>
    </div>
  </div>
  <button class="new-btn" @click="newColor">generate new color</button>

  <br />
  <span class="debug-btn" @click="isDebugging = !isDebugging">Debug use</span>
  <div v-if="isDebugging">
    <p>Item: {{ randomIdx }}</p>
    <p>Current Color: {{ currentColor }}</p>
    <p>Different Color: {{ diffColor }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Block from "./components/Block.vue";

const boardWidth = ref(4);
const currentColor = ref("#EEEEEE");
const diffColor = ref("#EDEDED");
const diffRate = ref(10);
const randomIdx = ref(
  Math.floor(Math.random() * (boardWidth.value * boardWidth.value) + 1)
);
const isDebugging = ref(false);
const isCorrect = ref(true);
const msg = ref("");

const newColor = () => {
  let g =
    Math.floor(Math.random() * (256 - diffRate.value * 2)) + diffRate.value;
  let r =
    Math.floor(Math.random() * (256 - diffRate.value * 2)) + diffRate.value;
  let b =
    Math.floor(Math.random() * (256 - diffRate.value * 2)) + diffRate.value;
  currentColor.value = "#" + r.toString(16) + g.toString(16) + b.toString(16);
  diffColor.value =
    "#" +
    Math.floor(r + Math.floor(Math.random() * diffRate.value)).toString(16) +
    Math.floor(g + Math.floor(Math.random() * diffRate.value)).toString(16) +
    Math.floor(b + Math.floor(Math.random() * diffRate.value)).toString(16);
  randomIdx.value = Math.floor(
    Math.random() * (boardWidth.value * boardWidth.value) + 1
  );
};

const handleCorrect = () => {
  msg.value = "Correct!";
  isCorrect.value = true;
  newColor();
};

const handleWrong = () => {
  msg.value = "Wrong!";
  isCorrect.value = false;
};

newColor();
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h2 {
  margin: 0;
}

.row {
  display: flex;
  justify-content: center;
}

.row-text {
  font-size: 20px;
  margin: 0 10px;
}

.row-selection {
  display: flex;
  justify-content: center;
  align-items: center;
}

.row-selector {
  padding: 6px 10px;
  margin: 8px 0;
  box-sizing: border-box;
  border-radius: 8px;
  border: 1px solid #232323;
}

.message {
  margin: 10px;
  font-size: 26px;
  height: 40px;
}

.correct {
  color: #66ff66;
}
.wrong {
  color: #ff3300;
}

.new-btn {
  border: none;
  background-color: lightgreen;
  height: 40px;
  width: 180px;
  border-radius: 20px;
  margin: 10px;
}

.debug-btn {
  cursor: pointer;
}
</style>
