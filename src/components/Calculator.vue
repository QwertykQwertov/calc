<template>
  <div class="container">
    <input
      type="text"
      @keydown.enter="result"
      v-model="value"
      name=""
      id=""
      placeholder="0"
    />
    <div class="wrapper">
      <button
        class="cell num"
        @click="input(num)"
        v-for="(num, key) in nums"
        :key="key"
      >
        {{ num }}
      </button>
      <button class="cell operand" @click="input('+')">+</button>
      <button class="cell num" @click="input('0')">0</button>
      <button
        class="cell operand"
        @click="input(operand)"
        v-for="operand in operands"
        :key="operand"
      >
        {{ operand }}
      </button>
      <button class="cell operand" @click="removeLast">C</button>
      <button class="cell operand" @click="reset">R</button>
      <button class="cell operand" @click="result">=</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      nums: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      operands: ["-", "*", "/", "(", ")"],
      value: "",
      lastSymbol: '',
      isOpen: false
    };
  },
  methods: {
    result() {
      if(!Number.isInteger(this.lastSymbol) && this.lastSymbol !== ")"){
        alert("Некорректный ввод!")
      }
      this.value = eval(this.value);
      this.value += "";
    },
    input(symbol) {
      // Добавить проверку на некорректный ввод +добавить computed() и replace недопустимые символы
      if (symbol === "("){
        if(this.isOpen === true) return
        this.isOpen = true
      } 
      if (symbol === ")"){
        if(this.isOpen === false) return
        this.isOpen = false
      } 
      this.lastSymbol = symbol
      this.value += symbol;
    },  
    reset() {
      this.value = "";
    },
    removeLast() {
      if(this.lastSymbol === ")") this.isOpen = true
      if(this.lastSymbol === "(") this.isOpen = false
      this.value = this.value.slice(0, this.value.length - 1)
      this.lastSymbol = this.value[this.value.length -1]
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  margin: 5%;
}
.wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  border: none;
}
.cell {
  flex: 33%;
  height: 50px;
  font-size: 18px;
  border: none;
}
.num {
  background-color: rgb(23, 45, 65);
  color: azure;
  border: 0.5px solid rgb(83, 78, 78);
}
.operand {
  background-color: rgb(70, 43, 21);
  color: azure;
  border: 0.5px solid rgb(83, 78, 78);
}
input {
  width: 80%;
  font-size: 18px;
  padding: 5px 5px;
  text-align: right;
  background-color: rgb(73, 68, 68);
  color: azure !important;
  border: none;
}
input::placeholder {
  color: azure;
}
</style>
