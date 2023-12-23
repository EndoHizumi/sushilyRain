<template>
  <div class="header">
    <h1>sushilyRain</h1>
    <h2>The rain will turn into sushi after midnight...</h2>
    <div>
      <label>sushi:</label>
      <button @click="addSushi">Add</button>
    </div>
    <div>
      <label>autoAdd:</label>
      <button @click="isAutoAdd = !isAutoAdd">{{ isAutoAdd ? 'stop' : 'start' }}</button>
    </div>
  </div>
  <div>
    <div v-for="rain in rains" :key="rain.id" class="box" :class="`box${rain.id}`" :style="rain.style"
      @animationend="rain.show = false" v-show="rain.show">
      🍣
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

let nextId = 0;
let intervalId = null;
const isAutoAdd = ref(true);
const rains = ref([]);
const addSushi = () => {
  const randomLeft = Math.random() * (window.innerWidth - 50); // 50はボックスの幅
  const newItem = {
    id: nextId++,
    show: true,
    style: {
      left: `${randomLeft}px`,
      animation: `moveDiagonally ${Math.random() * 10}s forwards`
    }
  };
  rains.value.push(newItem);
}
intervalId = setInterval(addSushi, 800)
const manageAutoAdd = () => {
  if (isAutoAdd.value) {
    intervalId = setInterval(addSushi, 800)
  } else {
    clearInterval(intervalId);
  }
}

watch(isAutoAdd, manageAutoAdd);
</script>

<style>
button {
  margin-left: 5px;
}

.box {
  position: absolute;
  top: 0px;
  z-index: 1;
  font-size: 50px;
}

.header {
  text-align: center;
  z-index: 999;
  position: relative;
}

body {
  background-color: #121212;
  color: rgb(205, 204, 204);
  overflow: hidden;
}

@keyframes moveDiagonally {

  to {
    transform: translate(calc(100vw - -50px), calc(100vh - 50px)) rotate(300deg);
  }
}
</style>
