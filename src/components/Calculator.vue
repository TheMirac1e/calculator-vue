<script>
import Button from "./Button.vue";

export default {
  data() {
    return {
      currentValue: '0',
      prevValue: '0',
      currentOperator: null,
    }
  },
  methods: {
    clearAll() {
      this.currentValue = '0';
      this.prevValue = '0';
      this.currentOperator = null;
    },

    clearOneChar() {
      if (this.currentValue !== '0') {
        this.currentValue = this.currentValue.toString().slice(0, -1);
      } else if (this.currentValue === '') {
        this.currentValue = '0'
      }
    },

    appendNumber(number) {
      if (number === '.' && this.currentValue.includes('.')) return;

      if(this.currentValue === '0') {
        this.currentValue = number.toString();
      } else {
        this.currentValue += number.toString();
      }
    },

    appendOperand(operand) {
      if (this.currentValue === '' && this.currentValue === '0') return;

      if(this.lastIsOperand()) {
        const str = this.prevValue.toString()
        const cur = str.slice(0, -1) + operand.toString();

        this.prevValue = cur;
      } else {
        this.prevValue = this.currentValue + operand;
        this.currentOperator = operand.toString();
        this.currentValue = '0';
      }
    },

    lastIsOperand() {
      const operand = this.prevValue.slice(-1);
      return operand === '+' || operand === '*' || operand === '/' || operand === '-';
    },

    compute() {
      let computation;
      const prev = parseFloat(this.prevValue);
      const current = parseFloat(this.currentValue);
      if (isNaN(prev) || isNaN(current)) return;

      switch (this.currentOperator) {
        case '+':
          computation = prev + current;
          break;
        case '*':
          computation = prev * current;
          break;
        case '/':
          computation = prev / current;
          break;
        case '-':
          computation = prev - current;
          break;
        default:
          return;
      }

      this.currentValue = computation;
      this.operand = null;
      this.prevValue = '';
    }
  },
  components: {
    Button
  }
}

</script>

<template>
  <div class="flex flex-col items-end bg-gray-900 p-2 mb-1 br-2 rounded-lg h-24 justify-end">
    <div class="text-lg mb-1">
      {{ prevValue }}
    </div>
    <div class="text-3xl">
      {{ currentValue }}
    </div>
  </div>
  <div class="grid grid-cols-4 gap-1">
    <Button value="AC" class-name="col-span-2" @click="clearAll()"/>
    <Button value="DEL" @click="clearOneChar()"/>
    <Button value="*" @click="appendOperand('*')"/>
    <Button value="7" @click="appendNumber(7)"/>
    <Button value="8" @click="appendNumber(8)"/>
    <Button value="9" @click="appendNumber(9)"/>
    <Button value="/" @click="appendOperand('/')"/>
    <Button value="4" @click="appendNumber(4)"/>
    <Button value="5" @click="appendNumber(5)"/>
    <Button value="6" @click="appendNumber(6)"/>
    <Button value="-" @click="appendOperand('-')"/>
    <Button value="1" @click="appendNumber(1)"/>
    <Button value="2" @click="appendNumber(2)"/>
    <Button value="3" @click="appendNumber(3)"/>
    <Button value="+" @click="appendOperand('+')"/>
    <Button value="0" @click="appendNumber(0)"/>
    <Button value="." @click="appendNumber('.')"/>
    <Button value="=" class-name="col-span-2" @click="compute()"/>
  </div>
</template>

<style scoped></style>
