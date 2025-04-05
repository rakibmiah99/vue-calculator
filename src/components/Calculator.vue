<template>
    <div class="calculator">
      <div class="display">{{display || '0'}}</div>
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
      <div @click="dot" class="btn">.</div>
      <div @click="equal" class="btn operator">=</div>
    </div>
  </template>
  
  <script setup>
    import { computed, ref } from 'vue';
    const previous = ref(null);
    const current = ref('');
    const operator = ref(null);
    const operatorClicked = ref(false);
    const operatorSymbol = ref(null);

    const display = computed(() => {
        if (previous.value) {
            return `${previous.value} ${operatorSymbol.value} ${current.value}`;
        } else if (current.value) {
            return current.value;
        }
        else {
            return '0';
        }
    });
    

    const clear = () => {
        current.value = '';
        previous.value = null;
        operator.value = null;
        operatorClicked.value = false;
        operatorSymbol.value = null;
    }

    const sign = () => {
        current.value = current.value.charAt(0) === '-' ? 
        current.value.slice(1) : `-${current.value}`;
    }

    const percent = () => {
        current.value = `${parseFloat(current.value) / 100}`;
    }
      
    const append = (number) => {
        if (operatorClicked.value) {
          current.value = '';
          operatorClicked.value = false;
        }
        current.value = `${current.value}${number}`;
    }
      
    const dot = () => {
        if (current.value.indexOf('.') === -1) {
          append('.');
        }
    }
      
    const setPrevious = () => {
        if(!current.value) {
            return;
        }
        previous.value = current.value;
        operatorClicked.value = true;
    }

    const setCurrentValuIsEmpty = () => {
        current.value = '';
    }
      
    const divide = () => {
        operator.value = (a, b) => b / a;
        operatorSymbol.value = '/';
        setPrevious();
        setCurrentValuIsEmpty();
    }
      
    const times = () => {
        operator.value = (a, b) => a * b;
        setPrevious();
        setCurrentValuIsEmpty();
        operatorSymbol.value = 'x';
    }
     
    const minus = () => {
        operator.value = (a, b) => a - b;
        setPrevious();
        setCurrentValuIsEmpty();
        operatorSymbol.value = '-';
    }
      
    const add = () => {
        operator.value = (a, b) => a + b;
        setPrevious();
        setCurrentValuIsEmpty();
        operatorSymbol.value = '+';
    }
      
    const equal = () => {
        const output = `${operator.value(
          parseFloat(current.value), 
          parseFloat(previous.value)
        )}`;

        if(output.split('.').length == 2){
            current.value = Number(output).toFixed(2);
        } else {
            current.value = output;
        }
        

        previous.value = null;
        operator.value = null;
        operatorClicked.value = false;
        operatorSymbol.value = null;
    }
    

  </script>
  
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
    grid-column: 1 / 5;
    background-color: #333;
    color: white;
  }
  
  .zero {
    grid-column: 1 / 3;
  }
  
  .btn {
    background-color: #F2F2F2;
    border: 1px solid #c0bcbc;
  }
  
  .operator {
    background-color: rgb(83, 8, 182);
    color: white;
  }
  </style>