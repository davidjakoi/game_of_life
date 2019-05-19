<template>
  <div>
    <div v-for="num in numbers" v-bind:key="num.id" :style="{ display: 'flex', flexDirection: 'row'}">
      <div class="cell" v-for="cell in num" v-bind:key="cell.id" :style="{ backgroundColor: cell === 0 ? 'white' : 'black'}" v-on:click="clickCell(key, key)">
        {{cell}}
      </div>
    </div>
  </div>
</template>

<script>
import { clearInterval } from 'timers';
export default {
  name: 'game',
  data () {
    return {
      numbers: [],
      rows: [],
      columns: [0,1,2,3,4,5,6,7,8,9]
    }
  },
  methods: {
    generateNumber() {
      return Math.round(Math.random());
    },
    checkNeighbors(y,x) {
      debugger
      let sum = 0;
      for(let topIndex = -1; topIndex < 2; topIndex++){
        if(y-1 >= 0 && x-1 >= 0 && this.numbers[y-1][x + (topIndex)] === 1){
          sum++;
        }
      }
      for(let sameIndex = -1; sameIndex < 2; sameIndex += 2){
        if(y-1 >= 0 && x-1 >= 0 && this.numbers[y][x + (sameIndex)] === 1){
          sum++;
        }
      }
      for(let bottomIndex = -1; bottomIndex < 2; bottomIndex++){
        if(y-1 >= 0 && x-1 >= 0 && y+1 < 10 && this.numbers[y+1][x + (bottomIndex)] === 1){
          sum++;
        }
      }
      /* console.log(sum); */
      return sum;
    },
    clickCell(num, cell) {
      console.log(this.numbers[num][cell]);
    }
  },
  mounted() {
    for (let columnIndex = 0; columnIndex < this.columns.length; columnIndex++) {
      this.numbers.push([]);
      for (let index = 0; index < 10; index++) {
        this.numbers[columnIndex].push(Math.round(Math.random()));
      }  
    };
    if(a){
      clearInterval(a);
    }
    const a = 
    setInterval(() => {
      /* console.log(this.numbers); */
      /* console.log('oksa'); */
      for (let columnIndex = 0; columnIndex < this.numbers.length; columnIndex++) {
        for (let index = 0; index < 10; index++) {
          /* console.log(this.checkNeighbors(columnIndex, index)); */
         /*  console.log(this.numbers[columnIndex][index]); */
          if(this.checkNeighbors(columnIndex, index) < 2){
           /*  console.log("meghal");
            this.numbers[columnIndex][index] = 0; */
            this.numbers[columnIndex].splice(index, 1, 0);
          } else if(this.checkNeighbors(columnIndex, index) > 3){
           /*  console.log("meghal"); */
            /* this.numbers[columnIndex][index] = 0; */
            this.numbers[columnIndex].splice(index, 1, 0);
          } else if(this.checkNeighbors(columnIndex, index) === 3 && this.numbers[columnIndex][index] === 0){
           /*  console.log("megszuletik"); */
           /*  this.numbers[columnIndex][index] = 1; */
            this.numbers[columnIndex].splice(index, 1, 0);
          }
        }  
      }
    }, 1000);
  }
  /*A sejt túléli a kört, ha két vagy három szomszédja van.
  A sejt elpusztul, ha kettőnél kevesebb (elszigetelődés), vagy háromnál több (túlnépesedés) szomszédja van.
  Új sejt születik minden olyan cellában, melynek környezetében pontosan három sejt található. */
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cell {
  width: 30px;
  height: 30px;
}
</style>
