<template>
  <div class="calculator">
    <div class="calculator-screen" ref="screen">
      <auto-shrink :value ="displayValue" @scale="scaled"></auto-shrink>
    </div>

    <button class="calculator-button calculator-function" @click="clearDisplay">C</button>
    <button class="calculator-button calculator-function"  @click="toggleSign">±</button>
    <button class="calculator-button calculator-function" @click="inputPercent">%</button>
    <button class="calculator-button calculator-operator" @click="performOperation('/')">÷</button>
    <button class="calculator-button calculator-number" @click="inputDigit(7)">7</button>
    <button class="calculator-button calculator-number" @click="inputDigit(8)">8</button>
    <button class="calculator-button calculator-number" @click="inputDigit(9)">9</button>
    <button class="calculator-button calculator-operator" @click="performOperation('*')">×</button>
    <button class="calculator-button calculator-number" @click="inputDigit(4)">4</button>
    <button class="calculator-button calculator-number" @click="inputDigit(5)">5</button>
    <button class="calculator-button calculator-number" @click="inputDigit(6)">6</button>
    <button class="calculator-button calculator-operator" @click="performOperation('-')">-</button>
    <button class="calculator-button calculator-number" @click="inputDigit(1)">1</button>
    <button class="calculator-button calculator-number" @click="inputDigit(2)">2</button>
    <button class="calculator-button calculator-number" @click="inputDigit(3)">3</button>
    <button class="calculator-button calculator-operator" @click="performOperation('+')">+</button>
    <!-- eslint-disable-next-line -->
    <button class="calculator-button calculator-number calculator-zero" @click="inputDigit(0)">0</button>
    <button class="calculator-button calculator-number" @click="inputDot">.</button>
    <button class="calculator-button calculator-operator" @click="performOperation('=')">=</button>
</div>
</template>

<script>
import AutoShrink from './AutoShrink';

export default {
  name: 'Calculator',
  components: {
    AutoShrink,
  },
  data() {
    return {
      value: null,
      displayValue: '0',
      waitingForOperand: false,
      operator: null,
    };
  },
  methods: {
    scaled(scale) {
      if (scale > 1) {
        this.$refs.screen.style.justifyContent = 'center';
      } else {
        this.$refs.screen.style.justifyContent = 'flex-end';
      }
    },
    inputDigit(digit) {
      if (this.waitingForOperand) {
        this.displayValue = String(digit);
        this.waitingForOperand = false;
      } else {
        this.displayValue = this.displayValue === '0' ? String(digit) : this.displayValue + digit;
      }
    },
    inputDot() {
      if (this.waitingForOperand) {
        this.displayValue = '.';
        this.waitingForOperand = false;
      } else if (!this.displayValue.includes('.')) {
        this.displayValue = `${this.displayValue}.`;
        this.waitingForOperand = false;
      }
    },
    clearDisplay() {
      this.displayValue = '0';
    },
    toggleSign() {
      this.displayValue = this.displayValue.startsWith('-') ? this.displayValue.substr(1) : `-${this.displayValue}`;
    },
    inputPercent() {
      const value = parseInt(this.displayValue, 10);
      this.displayValue = String(value / 100);
    },
    performOperation(nextOperator) {
      const next = parseFloat(this.displayValue);
      /*eslint-disable */
      const operations = {
        '/': (prev, next) => prev / next,
        '*': (prev, next) => prev * next,
        '+': (prev, next) => prev + next,
        '-': (prev, next) => prev - next,
        '=': (prev, next) => next,
      };
        /* eslint-enable */

      if (this.value === null) {
        this.value = next;
      } else if (this.operator) {
        const currentValue = this.value || 0;
        const computedValue = operations[this.operator](currentValue, next);
        this.value = computedValue;
        this.displayValue = String(computedValue);
      }

      this.waitingForOperand = true;
      this.operator = nextOperator;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
html {
    font-size: 62.5%;
}
body {
    margin: 0;
    font: 100 10px 'Roboto';
    font-weight: lighter;
}

.calculator {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: 130px auto;
    grid-gap: 1px 1px;
    margin: 50px auto;
    width: 348px;;
    height: 530px;
    background: #000;
    box-shadow: 0px 0px 20px 0px #aaa;
    border-radius: 3px;
}
.calculator-screen {
    display: flex;
   justify-content: flex-end;
    grid-column: 1 / -1;
    padding: 20px;
    color: #fff;
    font-size: 4.5rem;
}

.calculator-button {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 3.2rem;
    cursor: pointer;
    border: none;
}
button:active {
  box-shadow: inset 0px 0px 80px 0px rgba(0,0,0,0.25);
}
.calculator-function {
    background: #d9dbe1;
    color: #444;
}
.calculator-operator {
    background: #f97e0f;
    color: #fff;
}
.calculator-number {
    background: #d9dbe1;
       color: #444;
}
.calculator-zero {
    grid-column: 1 / 3;
}
</style>
