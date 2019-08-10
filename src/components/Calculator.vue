<template>
  <div class="calculator">
   <div class="display">{{current || '0'}}</div>
   <div @click="clear" class="btn">C</div>
   <div @click="sign" class="btn">+/-</div>
   <div @click="percent" class="btn">%</div>
   <div @click="divide" class="btn operator">÷</div>
   <div @click="append('7')" class="btn">7</div>
   <div @click="append('8')" class="btn">8</div>
   <div @click="append('9')" class="btn">9</div>
   <div @click="times" class="btn operator">x</div>
   <div @click="append('4')" class="btn">4</div>
   <div @click="append('5')" class="btn">5</div>
   <div @click="append('6')" class="btn">6</div>
   <div @click="minus" class="btn operator">-</div>
   <div @click="append('1')" class="btn">1</div>
   <div @click="append('2')" class="btn">2</div>
   <div @click="append('3')" class="btn">3</div>
   <div @click="add" class="btn operator">+</div>
   <div @click="append('0')" class="btn zero">0</div>
   <div @click="dot" class="btn ">.</div>
   <div @click="equal" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      previous: null,
      current: '',
      operator: null,
      operatorClicked: false
    }
  },
  methods: {
    clear() {
      this.current = '';
      this.operator = null;
      this.operatorClicked = false;
      this.previous = null;
    },
    sign() {
      this.calculatePrevious();
      this.current = this.current.charAt(0) === '-' ?
       this.current.slice(1): `-${this.current}`;
    },
    percent() {
      this.calculatePrevious();
      this.current = `${parseFloat(this.current)/1000}`
    },
    append(number){
      if(this.operatorClicked){
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    dot(){
      this.calculatePrevious();
      if(this.current.indexOf('.')===-1){
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    calculatePrevious() {
      if(this.operator&&this.previous){
        this.equal();
        this.operatorClicked = false;
      }
    },
    divide(){
      this.calculatePrevious();
      this.operator = (dividee, divider)=> dividee/divider;
      this.setPrevious();
    },
    times(){
      this.calculatePrevious();
      this.operator = (timesee, timeser)=> timesee*timeser;
      this.setPrevious();
    },
    minus(){
      this.calculatePrevious();
      this.operator = (minusee, minuser)=> minusee-minuser;
      this.setPrevious();
    },
    add(){
      this.calculatePrevious();
      this.operator = (addee, adder)=> addee + adder;
      this.setPrevious();
    },
    equal(){
      if (this.operator) {
        this.current = `${this.operator(
          parseFloat(this.previous),
          parseFloat(this.current)
        )}`;
        this.previous = null;
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .calculator {
    margin: 0 auto;
    width: 400px;
    font-size: 40px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
  }
  .display {
    background-color: #333;
    grid-column: 1 /5;
    color: white;
  }
  .zero {
    grid-column: 1/ 3;
  }
  .btn {
    background-color: #f2f2f2;
    border: 1px solid #999;
  }
  .operator {
    background-color: orange;
    color: white;
  }
</style>
