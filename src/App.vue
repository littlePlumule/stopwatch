<script setup>
import { ref } from 'vue'
import { IonIcon } from '@ionic/vue'

const min = ref('00')
const sec = ref('00')
const cs = ref('00')
const timer = ref(null)
const iconName = ref('play')
const disabled = ref(false)
const btnClass = ref({
  play: true,
  pause: false,
})

function toggle() {
  if (iconName.value === 'play') {
    startTimer();
    iconName.value = 'pause';
    btnClass.value.play = false;
    btnClass.value.pause = true;
    disabled.value = true;
  } else {
    clearTimeout(timer.value);
    iconName.value = 'play';
    btnClass.value.play = true;
    btnClass.value.pause = false;
    disabled.value = false;
  }        
}

function startTimer() {
  timer.value = setInterval(() => {
    cs.value++;
    if (cs.value < 10) {
      cs.value = `0${cs.value}`;
    }
    if (cs.value == 100) {
      sec.value++;
      if (sec.value < 10) {
        sec.value = `0${sec.value}`;
      }
      cs.value = '00';
    }
    if (sec.value == 60) {
      min.value++;
      if (min.value < 10) {
        min.value = `0${min.value}`;
      }
      sec.value = '00';
    }
    if (min.value == 60) {
      clearTimeout(timer.value);
    }
  }, 10);
}

function reset() {
  min.value = '00';
  sec.value = '00';
  cs.value = '00';
  timer.value = null;
}
</script>

<template>
  <div class="stopwatch">
    <div class="title">online stopwatch</div>
    <div class="timer" v-cloak>
      <div class="min">{{ min }}</div>
      <span>:</span>
      <div class="sec">{{ sec }}</div>
      <span>.</span>
      <div class="cs">{{ cs }}</div>
    </div>
    <div class="control">
      <button 
        @click="toggle" 
        :class="btnClass">
        <ion-icon :name="iconName"></ion-icon>
      </button>
      <button 
        @click="reset" 
        class="reset" 
        :disabled="disabled">
        <ion-icon name="refresh"></ion-icon>
      </button>
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  list-style: none;
  box-sizing: border-box;
}

html, body {
  width: 100%;
  height: 100%;
}

#app {
  width: 100%;
  height: 100%;
  background-color: #f8f8f8;
}

.stopwatch {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  height: 500px;
  width: 500px;
}

.title {
  font-size: 36px;
  font-weight: bold;
  color: #fa728e;
}

.timer {
  display: flex;
  font-size: 76px;
  margin: 20px 0;
  color: #323232;
}

.min, .sec, .cs {
  width: 80px;
}

span {
  margin: 0 10px;
}

.control {
  display: flex;
}

button {
  cursor: pointer;
  background-color: transparent;
  border: none;
  border-radius: 50%;
  padding: 0;
  font-size: 24px;
  width: 70px;
  height: 70px;
  transition: background .2s;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff
}

button:hover {
  box-shadow: inset 0 0 10px rgba(50, 50, 50, .5);
}

.play {
  background-color: #16a69c;
}

.pause {
  background-color: #fa728e;
}

.reset {
  background-color: #777777;    
}

.reset:disabled {
  cursor: not-allowed;
  box-shadow: none;
}

button + button {
  margin-left: 40px;
}
</style>
