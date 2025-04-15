<template>
  <q-input v-model="l" class="l-inp" rounded outlined label="Введите длину рельсы в см" />
  <q-input v-model="coord" class="def-coor"rounded outlined label="Введите координаты дефектов" />
  <q-btn @click="getInp" class="scan-button" outline rounded color="primary" label="Сканировать" />
  <div class="relsa">
    <div class="deffects" v-for="coord in coordM" :style="{ 'left' : `${ coord in coordM } px`}">
    </div>

    <div class="scan-line" :style="{ 'left' : `${ scanPos }px`}">
    </div>
  </div>

</template>

<script setup>
import { ref } from 'vue'

let l = ref('')
let coord = ref('')
let scanPos = ref(0)
let coordM = ref([])

function getInp() {
  coordM = coord.value.split(' ')
  if (l.value === ''){
    alert('Введите длину рельсы')
  }
  if (coord.value == ''){
    alert('Введите координаты дефектов')
  }
  console.log(window.innerWidth)

  let interval = setInterval(() => {
    scanPos.value = scanPos.value + 10

    if (scanPos.value >= window.innerWidth - 10) {
      clearInterval(interval)
    }
  }, 400);

}
</script>

<style>
.l-inp{
  width: 300px;
  margin: 20px;
}

.def-coor{
  width: 300px;
  margin: 20px;
}

.scan-button{
  margin: 20px;
}

.relsa{
  width: 100%;
  height: 150px;
  background-color: gray;
  position: relative;
}

.scan-line{
  width: 10px;
  height: 150px;
  background: red;
  position: absolute;
  left: 0px;
}

.deffects{
  height: 20px;
  width: 20px;
  background: lawngreen;
  border-radius: 50%;
  position: absolute;
}
</style>
