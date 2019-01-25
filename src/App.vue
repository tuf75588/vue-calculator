<template lang="html">
  <main class="calculator">
    <div ref="display" class="display">
      <div class="display-text">
        <span ref="displayText">{{display}}</span>
      </div>
    </div>
    <div class="buttons">
      <div class="button-row" v-for="row in buttonRows">
        <div
          :class="{ operator: button.type == 'operator' }"
          :style="button.style"
          class="button"
          v-for="button in row"
          v-on:click="buttonClick(button)"

          >
          {{button.text}}
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: 'app',
  methods: {
    buttonClick(button) {
      if (button.type === 'number') {
        if (this.previousValue === null) {
          this.previousValue = Number(this.display);
          this.display = '';
        }
        this.display += button.text;
      } else if (button.text === 'AC') {
        this.display = '';
      } else if (button.text === '.') {
        if (this.display.includes('.') || this.display === '') return;
        else {
          this.display = this.display + button.text;
        }
      } else if (button.text === '+/-') {
        this.display *= -1;
      } else if (button.type === 'operator') {
        if (this.currentOperator) {
          const operationType = this.operations[this.currentOperator];
          this.display = operationType(
            this.previousValue,
            parseInt(this.display)
          );
        }
        this.previousValue = null;
        this.currentOperator = button.text;
      } else if (button.text === '=') {
        const evaluator = this.operations[this.currentOperator];
        this.currentOperator = '';
        this.display = evaluator(+this.previousValue, +this.display);
      }
    },
  },
  data: () => ({
    currentOperator: '',
    previousValue: null,
    total: null,
    operations: {
      '÷': (a, b) => (a % b !== 0 ? (a / b).toFixed(1) : a / b),
      '×': (a, b) => a * b,
      '-': (a, b) => a - b,
      '+': (a, b) => a + b,
    },
    display: '',
    buttonRows: [
      [
        { text: 'AC', type: 'special' },
        { text: '+/-', type: 'special' },
        { text: '😊', type: 'special' },
        { text: '÷', type: 'operator' },
      ],
      [
        { text: '7', type: 'number' },
        { text: '8', type: 'number' },
        { text: '9', type: 'number' },
        { text: '×', type: 'operator' },
      ],
      [
        { text: '4', type: 'number' },
        { text: '5', type: 'number' },
        { text: '6', type: 'number' },
        { text: '-', type: 'operator' },
      ],
      [
        { text: '1', type: 'number' },
        { text: '2', type: 'number' },
        { text: '3', type: 'number' },
        { text: '+', type: 'operator' },
      ],
      [
        { text: '0', type: 'number', style: 'flex-basis: calc(100%/2)' },
        { text: '.', type: 'special', style: 'flex-basis: calc(100%/2)' },
        { text: '=', type: 'special' },
      ],
    ],
  }),
  components: {},
};
</script>

<style>
body {
  width: 100vw;
  height: 100vh;
  font-size: 2.5vw;
  display: flex;
  justify-content: center;
  align-items: center;
}

.calculator {
  width: 30vw;
  height: calc(30vw * 1.4);
  font-family: sans-serif;
  border-radius: 5px;
  overflow: hidden;
  border: 0.5px solid grey;
}
.display {
  height: calc(30vw * 0.2);
  background: #989898;
  color: white;
  font-size: 4vw;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
.display-text {
  padding: 0.5vw;
  text-align: right;
}
.button-row {
  display: flex;
  height: 20%;
  width: 100%;
  justify-content: space-around;
}
.buttons {
  text-align: center;
  height: calc(30vw * 1.2);
}
.button {
  display: inline-block;
  outline: 0.5px solid grey;
  width: 100%;
  background: #d0d0d0;
  display: flex;
  justify-content: center;
  align-items: center;
}
.operator {
  background: #f6872c;
  color: #fff;
}
.button:hover {
  background: hsl(220, 24%, 93%);
  cursor: pointer;
}
.button:active {
  background: #b5b4b3;
}
</style>
