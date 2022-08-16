<template>
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="./assets/logo.svg"
      width="125"
      height="125"
    />
    <p>VueJS Calculator</p>
  </header>

  <div class="container">
    <div class="inputResult">
      <p>{{ calcResult }}</p>
    </div>
    <div class="calcButtons">
      <IndivButton
        :symbol="icon"
        v-for="icon in buttonsArray"
        :key="icon"
        @computeInput="computeInput"
      />
    </div>
  </div>
</template>

<script lang="ts">
import IndivButton from '@/components/IndivButton.vue';
import { defineComponent } from 'vue';
import { evaluate } from 'mathjs';

export default defineComponent({
  data(): {
    calcResult: string;
    calcArgs: string;
    resetCalc: boolean;
    buttonsArray: [string];
  } {
    return {
      calcResult: '0',
      calcArgs: '',
      resetCalc: false,
      buttonsArray: [
        'CE',
        '+/-',
        '/',
        '*',
        '-',
        '+',
        '7',
        '8',
        '9',
        '4',
        '5',
        '6',
        '1',
        '2',
        '3',
        '0',
        '.',
        '=',
      ],
    };
  },
  methods: {
    computeInput(x: string) {
      switch (x) {
        case 'CE': {
          this.calcResult = '0';
          this.calcArgs = '';
          break;
        }
        case '.': {
          if (!this.calcResult.includes('.')) {
            this.calcResult = this.calcResult + x;
          }
          break;
        }
        case '+/-': {
          if (!this.calcResult.includes('-')) {
            this.calcResult = '-' + this.calcResult;
          } else if (this.calcResult.includes('-')) {
            this.calcResult = this.calcResult.substring(1);
          }
          break;
        }
        case '0':
        case '1':
        case '2':
        case '3':
        case '4':
        case '5':
        case '6':
        case '7':
        case '8':
        case '9': {
          if (this.resetCalc) {
            this.calcResult = x;
            this.resetCalc = false;
            break;
          }
          if (
            this.calcResult == '0' ||
            this.calcResult == '+' ||
            this.calcResult == '-' ||
            this.calcResult == '/' ||
            this.calcResult == '*'
          ) {
            this.calcResult = x;
          } else if (this.calcResult == '-0') {
            this.calcResult = '-' + x;
          } else {
            this.calcResult = this.calcResult + x;
          }
          break;
        }
        case '+':
        case '/':
        case '-':
        case '*': {
          this.calcArgs = this.calcArgs + this.calcResult + x;
          this.calcResult = x;
          break;
        }
        case '=': {
          this.calcArgs = this.calcArgs + this.calcResult;
          this.resetCalc = true;
          if (evaluate(this.calcArgs).length > 15) {
            this.calcResult = evaluate(this.calcArgs).toFixed(5);
          } else {
            this.calcResult = evaluate(this.calcArgs).toString();
          }
          this.calcArgs = '';
          break;
        }
        default:
          break;
      }
    },
  },
  components: {
    IndivButton,
  },
});
</script>
