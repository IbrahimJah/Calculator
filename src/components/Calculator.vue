<template>
  <div class="calculator">
    <!--using div to create it seems easier than with tables-->
    <div class="display">{{ response }}</div>
    <!--display-->

    <div @click="percent" class="percent button">%</div>
    <div @click="clear" class="clear button">Clear</div>
    <div @click="append('7')" class="button">7</div>
    <div @click="append('8')" class="button">8</div>
    <div @click="append('9')" class="button">9</div>
    <div @click="multiply" class="button">x</div>
    <div @click="append('4')" class="button">4</div>
    <div @click="append('5')" class="button">5</div>
    <div @click="append('6')" class="button">6</div>
    <div @click="minus" class="button">-</div>
    <div @click="append('1')" class="button">1</div>
    <div @click="append('2')" class="button">2</div>
    <div @click="append('3')" class="button">3</div>
    <div @click="plus" class="button">+</div>
    <div @click="divide" class="button">/</div>
    <div @click="append('0')" class="button">0</div>
    <div @click="dot" class="button">.</div>
    <div @click="equal" class="button">=</div>
  </div>
</template>

<script>
export default {
  //display result
  data() {
    return {
      previous: null,
      response: "",
      operator: null,
      operatorClicked: false,
    };
  },

  //button function and logic
  methods: {
    clear() {
      this.response = '';
    },

    percent() {
      this.response = `${parseFloat(this.response) / 100}`; //parseFloat allows stings to be converted into a floating point, ${} is a template literal that lets me insert variables/ expressions
    },

    dot() {
      //the if statement will not add another dot if there is one already
      if (this.response.indexOf(".") === -1) {
        this.append(".");
      }
    },

    //a method to add to append string
    append(number) {
      //empty display if we clicked on an operator
      if (this.operatorClicked) {//if true then clear else set to false
        this.response = "";
        this.operatorClicked = false;
      }
      //append numbers clicked
      this.response = this.response + number;
    },

    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },

    multiply() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },

    minus() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },

    plus() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },

    setPrevious() {
      this.operatorClicked = true;
      this.previous = this.response;
    },

    equal() {
      //takes the current value and runs it against the previous
      this.response = `${this.operator(
        parseFloat(this.response),
        parseFloat(this.previous)
      )}`;
      this.previous = null;
    },
  },
};
</script>

<style scoped>
.calculator {
  width: 350px;
  margin: 0 auto; /*center the calculator*/
  padding: 50px;
  text-align: center;
  font-size: 30px;
  display: grid; /*display the divs in a grid format*/
  grid-template-columns: repeat(4, 1fr); /*repeat function to create 4 rows*/
  grid-auto-rows: minmax(
    50px,
    auto
  ); /*supposed to be better than using % to
  adjust the size*/
}
.display {
  background-color: #9bacd5;
  border: 3px solid #000000;
  text-align: right;
  grid-column: 1/5; /*with this the display will take up 4 columns (1 start, 5 end)*/
}
.percent {
  grid-column: 1/3;
}
.clear {
  grid-column: 3/5;
}
.button {
  background-color: #4e5e83;
  border: 2px solid #000000;
}
</style>
