<template>
  <h3>Поиск дефектов на рельсе</h3>
  <p>Данная программа выявляет наличие дефектов на рельсе</p>
  <p>Для поиска дефектов укажите длину рельсы (в см) и координаты дефектов (например - 10 20 30)</p>
  <p>Красный свет - дефект есть от 0 до 5 см</p>
  <p>Жёлтый свет - дефект есть от 5 до 10 см</p>
  <p>Зелёный свет - дефекта нет</p>
  <q-input v-model="l" class="l-inp" color="deep-orange" rounded outlined label="Введите длину рельсы в см" />
  <q-input v-model="coord" class="def-coor" color="deep-orange" rounded outlined label="Введите координаты дефектов" />
  <q-btn @click="getInp" class="scan-button" outline rounded color="deep-orange" label="Сканировать" />
  <div class="relsa">
<!--    <div :class = "{ 'redball' : coordSc.left === 'Красный свет (дефект 0 - 5)', 'greenball': coordSc.left === 'Зелёный свет (дефектов нет)', 'yellowball': coordSc.left === 'Жёлтый свет (дефект 5 - 10)'}" v-for = 'coordSc in FoundDiffects' :style="{ 'left' : `${ coordSc.x - 10 }px`}" >-->
<!--    </div>-->
<!--    <div :class = "{ 'redball' : coordSc.right === 'Красный свет (дефект 0 - 5)', 'greenball': coordSc.right === 'Зелёный свет (дефектов нет)', 'yellowball': coordSc.right === 'Жёлтый свет (дефект 5 - 10)'}" v-for = 'coordSc in FoundDiffects' :style="{ 'left' : `${ coordSc.x + 10 }px`}" >-->
<!--    </div>-->
    <div class="deffects" v-for = "coord in coordM" :style="{ 'left' : `${ coord }px`}">
    </div>

    <div class="scan-line" :style="{ 'left' : `${ scanPos }px`}">
    </div>
  </div>


  <ul>
    <li v-for = '(coordsc, index) in coordScan'>
      <div>
        Шаг номер {{ index + 1}}: Позиция {{ Perevod(coordsc) }} см
      </div>
      <div>
        Слева: {{ coordsc.left }} <span :class = "{ 'redball' : coordsc.left === 'Красный свет (дефект 0 - 5)', 'greenball': coordsc.left === 'Зелёный свет (дефектов нет)', 'yellowball': coordsc.left === 'Жёлтый свет (дефект 5 - 10)'}" >
      </span>
      </div>
      <div>
        Справа: {{ coordsc.right }} <span :class = "{ 'redball' : coordsc.right === 'Красный свет (дефект 0 - 5)', 'greenball': coordsc.right === 'Зелёный свет (дефектов нет)', 'yellowball': coordsc.right === 'Жёлтый свет (дефект 5 - 10)'}" >
      </span>
      </div>
    </li>
  </ul>

</template>

<script setup>
import { ref } from 'vue'

let l = ref('')
let coord = ref('')
let scanPos = ref(0)
let coordM = ref([])
let coordScan = ref([])
let FoundDiffects = ref([])

let windowWidth = window.innerWidth - 25

function Perevod(coordsc){
  return coordsc.x * l.value / windowWidth
}

function getInp() {
  let tenSm = windowWidth * 10 / +l.value

  coordScan.value = []
  scanPos.value = 0
  coordM.value = coord.value.split(' ')
  for (let i = 0; i < coordM.value.length; i++) {
    coordM.value[i] = windowWidth * +coordM.value[i] / +l.value
  }
  if (l.value === ''){
    alert('Введите длину рельсы')
  }
  if (coord.value == ''){
    alert('Введите координаты дефектов')
  }

  let interval = setInterval(() => {
    scanPos.value = scanPos.value + windowWidth * 1 / +l.value

    let newCoord = {
      x: scanPos.value,
      left: "Зелёный свет (дефектов нет)",
      right: "Зелёный свет (дефектов нет)",
    }

    newCoord.left = 'Красный свет (дефект 0 - 5)'
    FoundDiffects.value.push(newCoord)

    for (let coord of coordM.value) {
      // if (scanPos.value - tenSm <= +coord && +coord <= scanPos.value + tenSm) {
      //   if (scanPos.value - tenSm/2 <= +coord){
      //     newCoord.left = 'Красный свет (дефект 0 - 5)';
      //   }
      //   else if (scanPos.value - tenSm/2 >= +coord && +coord >= scanPos.value - tenSm) {
      //     newCoord.left = 'Жёлтый свет(дефект 5 - 10)';
      //   }
      //   if (+coord <= scanPos.value + tenSm/2){
      //     newCoord.right = 'Красный свет (дефект 0 - 5)'
      //   }
      //   else if (scanPos.value + tenSm/2 <= +coord && +coord <= scanPos.value + tenSm){
      //     newCoord.right = 'Жёлтый свет (дефект 5 - 10)'
      //   }
      // }
          const distance = Math.abs(scanPos.value - coord);

          if (distance <= tenSm/2) {
            newCoord.left = 'Красный свет (дефект 0 - 5)';
            break
          }
          else if (distance > tenSm/2 && distance <= tenSm) {
            newCoord.left = 'Жёлтый свет (дефект 5 - 10)';
          }

          if (distance <= tenSm/2) {
            newCoord.right = 'Красный свет (дефект 0 - 5)';
          }
          else if (distance > tenSm/2 && distance <= tenSm) {
            newCoord.right = 'Жёлтый свет (дефект 5 - 10)';
          }
    }

    coordScan.value.push(newCoord)

    if (scanPos.value >= windowWidth - 10) {
      clearInterval(interval)
    }
  }, 200);
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
  margin-bottom: 50px;
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

.redball{
  width: 20px;
  height: 20px;
  background: red;
  border-radius: 50%;
  display: inline-block;
}

.greenball{
  width: 20px;
  height: 20px;
  background: green;
  border-radius: 50%;
  display: inline-block;
}

.yellowball{
  width: 20px;
  height: 20px;
  background: yellow;
  border-radius: 50%;
  display: inline-block;
}

p{
  margin-left: 5px;
}
</style>
