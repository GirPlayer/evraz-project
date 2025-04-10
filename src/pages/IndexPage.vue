<template>
  <q-input v-model="l" class="l-inp" rounded outlined label="Введите длину рельсы в см" />
  <q-input v-model="coord" class="def-coor"rounded outlined label="Введите координаты дефектов" />
  <q-btn @click="getInp" class="scan-button" outline rounded color="primary" label="Сканировать" />
  <div class="relsa">
    <div class="scan-line">
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

let l = ref('')
let coord = ref('')

function getInp() {
  let coordM = coord.value.split(' ')
  if (l.value === ''){
    alert('Введите длину рельсы')
  }
  if (coord.value == ''){
    alert('Введите координаты дефектов')
  }
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
}

.scan-line{
  width: 10px;
  height: 150px;
  background: red;
}
</style>
