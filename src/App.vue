<script>
import Button from "./components/Button.vue";
export default {
  name: "App",
  components: {
    Button,
  },
  data() {
    return {
      initialValue: "0",
      savedValue: 0,
      operator: "",
      theme: 1,
      inputs:[ 
          7,8,9,"del",
          4,5,6,"+",
          1,2,3,"-",
          ".","0","/","x",
          "reset","equal"]
    };
  },
  methods: {
    getValue(e) {
      let value = e.target.value;
      if (value) {
        if (value == "del") {
          this.del();
        } else if (value == "reset") {
          this.reset();
        } else if (value == "equal") {
          this.equal();
        } else if (
          value == "+" ||
          value == "-" ||
          value == "x" ||
          value == "/"
        ) {
          this.operation(value);
        } else {
          this.addNum(value);
        }
      }
    },
    addNum(value) {
      if (this.initialValue == "0") {
        this.initialValue = value;
      } else this.initialValue += value;
    },
    del() {
      if (this.initialValue.length === 1) {
        this.initialValue = "0";
      } else {
        this.initialValue = this.initialValue.slice(0, -1);
      }
    },
    reset() {
      this.initialValue = "0";
    },
    operation(value) {
      this.savedValue = Number(this.initialValue);
      this.initialValue = "0";
      this.operator = value;
    },
    calculate:(a,str,b)=>{
       switch(str){
          case '+': return a + b;
          case '-': return a - b;
          case '/': return  a / b;
          case 'x': return a * b;
          default: return '0';
        }
    },
    equal() {
	this.initialValue = this.calculate(this.savedValue, this.operator, Number(this.initialValue));      
    },
    changeTheme() {
      this.theme++;
      if (this.theme == 4) {
        this.theme = 1;
      }
      if (this.theme == 1) {
        document.body.classList.remove("theme3");
      } else if (this.theme == 2) {
        document.body.classList.add("theme2");
      } else if (this.theme == 3) {
        document.body.classList.remove("theme2");
        document.body.classList.add("theme3");
      }
    },
  },
  computed: {
    screenValue() {
	return this.initialValue
		.toString()
		.replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",");
    },
  },
};
</script>

<template>
  <div class="container">
    <header class="container__header">
      <h1>calc</h1>
      <div class="header-controls">
        <p>THEME</p>
        <div class="controls-container">
          <div class="header-themes">
            <p>1</p>
            <p>2</p>
            <p>3</p>
          </div>
          <div class="switch-container" @click="changeTheme">
            <div class="switch-circle"></div>
          </div>
        </div>
      </div>
    </header>
    <div class="container__screen">{{ screenValue }}</div>
    <div class="container__buttons" @click="getValue"> 
      <Button 
	  v-for="(input, i) in inputs"
	  :key="i"
	  :text="input"
	  :class="input.toString().lenght>1?`btn ${input} uppercase`:'btn'"
	  :value="input" 
      />   
    </div>
  </div>
</template>

<style>
@import "../public/styles.css";
@import url("https://fonts.googleapis.com/css2?family=Spartan:wght@700&display=swap");
#app {
  font-family: "Spartan", sans-serif;
  font-size: 2rem;
  color: #fff;
}
body {
  background: var(--th1-main-bg);
  padding: 24px;
  transition: all 0.5s ease-in-out;
}
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  margin: 0 auto;
  max-width: 540px;
}
.container__header {
  display: flex;
  justify-content: space-between;
}
.container__header h1 {
  font-size: 32px;
}
.header-controls {
  display: flex;
  font-size: 1rem;
  align-items: center;
}
.header-controls p {
  align-self: flex-end;
  font-size: 14px;
}
.header-themes {
  display: flex;
  justify-content: space-between;
}
.header-themes p {
  margin-left: 16px;
}
.controls-container {
  margin-left: 24px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.switch-container {
  width: 72px;
  height: 26px;
  background: var(--th1-screen-bg);
  border-radius: 1rem;
  margin: 8px 0 0 16px;
  display: flex;
  align-items: center;
  position: relative;
  cursor: pointer;
}
.switch-circle {
  width: 1rem;
  height: 1rem;
  background: var(--th1-red);
  border-radius: 50%;
  position: absolute;
  left: 4px;
  transition: all 0.5s ease-in-out;
}
.container__screen {
  width: 100%;
  background: var(--th1-screen-bg);
  padding: 26px;
  text-align: right;
  border-radius: 0.5rem;
  margin: 24px 0;
  overflow: auto;
}
.container__buttons {
  display: grid;
  border-radius: 0.5rem;
  grid-template: repeat(5, 1fr) / repeat(4, 1fr);
  gap: 14px;
  background: var(--th1-toggle-bg);
  padding: 24px;
}
.uppercase {
  text-transform : uppercase;
}
.btn {
  min-width: 40px;
  height: 60px;
  width: 100%;
  font-weight: 700;
  border: none;
  border-radius: 0.3rem;
  background: var(--th1-light-orange);
  box-shadow: 0 4px var(--th1-grayish-orange);
  cursor: pointer;
}
.btn:active {
  opacity: 0.9;
}
.reset {
  grid-column: 1 / 3;
  font-size: 1rem;
}
.equal {
  grid-column: 3 / 5;
  font-size: 1rem;
  color: #fff;
  background: var(--th1-red);
  box-shadow: 0 4px var(--th1-dark-red);
}
.del,
.reset {
  color: #fff;
  font-size: 1rem;
  background: var(--th1-desaturated-blue);
  box-shadow: 0 4px var(--th1-desaturated-blue-shadow);
}
.theme2 {
  background: var(--th2-main-bg);
}
.theme2 h1,
.theme2 p {
  color: var(--th2-dark-grayish-yellow);
}
.theme2 .switch-container {
  background: var(--th2-toggle-bg);
}
.theme2 .switch-circle {
  background: var(--th2-orange);
  transform: translateX(23px);
}
.theme2 .container__screen {
  background: var(--th2-screen-bg);
  color: var(--th2-dark-grayish-yellow);
}
.theme2 .container__buttons {
  background: var(--th2-toggle-bg);
}
.theme2 .btn {
  background: var(--th2-grayish-yellow);
  box-shadow: 0 4px var(--th2-grayish-orange);
  color: var(--th2-dark-grayish-yellow);
}

.theme2 .del,
.theme2 .reset {
  color: #fff;
  background: var(--th2-key-bg);
  box-shadow: 0 4px var(--th2-key-shadow);
}
.theme2 .equal {
  color: #fff;
  background: var(--th2-orange);
  box-shadow: 0 4px var(--th2-dark-orange);
}
.theme3 {
  background: var(--th3-main-bg);
}
.theme3 h1,
.theme3 p {
  color: var(--th3-light-yellow);
}
.theme3 .switch-container {
  background: var(--th3-toggle-bg);
}
.theme3 .switch-circle {
  background: var(--th3-pure-cyan);
  transform: translateX(46px);
}
.theme3 .container__screen {
  background: var(--th3-toggle-bg);
  color: var(--th3-light-yellow);
}
.theme3 .container__buttons {
  background: var(--th3-toggle-bg);
}
.theme3 .btn {
  color: var(--th3-light-yellow);
  background: var(--th3-very-dark-violet);
  box-shadow: 0 4px var(--th3-dark-magenta);
}

.theme3 .del,
.theme3 .reset {
  color: #fff;
  background: var(--th3-dark-violet);
  box-shadow: 0 4px var(--th3-magenta);
}
.theme3 .equal {
  color: var(--th3-dark-blue);
  background: var(--th3-pure-cyan);
  box-shadow: 0 4px var(--th3-soft-cyan);
}
</style>
