<template>
  <div class="calculator">
    <button @click="changeModeEvent" class="toggle-button">
      <span v-if="changeMode">普通计算器模式</span>
      <span v-else>科学计算器模式</span>
    </button>
    <div class="results">
      <input class="input" v-model="current" />
      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
    </div>
    <div class="mode" v-if="changeMode">
      <button class="button" @click="press">7</button>
      <button class="button" @click="press">8</button>
      <button class="button" @click="press">9</button>
      <button class="button" @click="press">*</button>
      <button class="button" @click="press">&#60;=</button>
      <button class="button" @click="press">C</button>
      <button class="button" @click="press">4</button>
      <button class="button" @click="press">5</button>
      <button class="button" @click="press">6</button>
      <button class="button" @click="press">/</button>
      <button class="button" @click="press">(</button>
      <button class="button" @click="press">)</button>
      <button class="button" @click="press">1</button>
      <button class="button" @click="press">2</button>
      <button class="button" @click="press">3</button>
      <button class="button" @click="press">-</button>
      <button class="button" @click="press">x ²</button>
      <button class="button" @click="press">±</button>
      <button class="button" @click="press">0</button>
      <button class="button" @click="press">.</button>
      <button class="button" @click="press">%</button>
      <button class="button" @click="press">+</button>
      <button class="button equal-sign" @click="press">=</button>
    </div>
    <div class="mode" v-else>
      <button class="button" @click="press">sin</button>
      <button class="button" @click="press">cos</button>
      <button class="button" @click="press">tan</button>
      <button class="button" @click="press">x^</button>
      <button class="button" @click="press">&#60;=</button>
      <button class="button" @click="press">C</button>
      <button class="button" @click="press">log</button>
      <button class="button" @click="press">ln</button>
      <button class="button" @click="press">e</button>
      <button class="button" @click="press">∘</button>
      <button class="button" @click="press">rad</button>
      <button class="button" @click="press">√</button>
      <button class="button" @click="press">7</button>
      <button class="button" @click="press">8</button>
      <button class="button" @click="press">9</button>
      <button class="button" @click="press">/</button>
      <button class="button" @click="press">x ²</button>
      <button class="button" @click="press">x !</button>
      <button class="button" @click="press">4</button>
      <button class="button" @click="press">5</button>
      <button class="button" @click="press">6</button>
      <button class="button" @click="press">*</button>
      <button class="button" @click="press">(</button>
      <button class="button" @click="press">)</button>
      <button class="button" @click="press">1</button>
      <button class="button" @click="press">2</button>
      <button class="button" @click="press">3</button>
      <button class="button" @click="press">-</button>
      <button class="button" @click="press">%</button>
      <button class="button" @click="press">±</button>
      <button class="button" @click="press">0</button>
      <button class="button" @click="press">.</button>
      <button class="button" @click="press">&#x003C0;</button>
      <button class="button" @click="press">+</button>
      <button class="button equal-sign" @click="press">=</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      current: '',
      changeMode: true,
      errorMessage: ''
    }
  },
  methods: {
    press(event) {
      let me = this
      let key = event.target.textContent

      // Clear any previous error message
      me.errorMessage = ''

      if (
        key !== '=' &&
        key !== 'C' &&
        key !== '*' &&
        key !== '/' &&
        key !== '√' &&
        key !== "x ²" &&
        key !== "%" &&
        key !== "<=" &&
        key !== "±" &&
        key !== "sin" &&
        key !== "cos" &&
        key !== "tan" &&
        key !== "log" &&
        key !== "ln" &&
        key !== "x^" &&
        key !== "x !" &&
        key !== "π" &&
        key !== "e" &&
        key !== "rad" &&
        key !== "∘"
      ) {
        me.current += key
      } else if (key === '=') {
        try {
          // Validate the expression before evaluation
          if (!this.isValidExpression(me.current)) {
            throw new Error('无效的表达式')
          }

          if (me.current.indexOf('^') > -1) {
            let [base, exponent] = me.current.split('^')
            me.current = Math.pow(parseFloat(base), parseFloat(exponent)).toString()
          } else {
            // Use Function instead of eval for better security and to catch syntax errors
            me.current = new Function('return ' + me.current)().toString()
          }

          // Check if the result is a valid number
          if (isNaN(me.current) || !isFinite(me.current)) {
            throw new Error('计算结果无效')
          }
        } catch (error) {
          me.errorMessage = error.message || '无效的表达式'
          // me.current = ''
        }
      } else if (key === 'C') {
        me.current = ''
        me.errorMessage = '' // Also clear any error message
      } else if (key === '*' || key === '/' || key === '+' || key === '-') {
        // Prevent multiple consecutive operators
        if (this.isOperator(me.current.slice(-1))) {
          me.errorMessage = '不能连续输入多个运算符'
        } else {
          me.current += key
        }
      } else if (key === '±') {
        if (me.current.charAt(0) === '-') {
          me.current = me.current.slice(1)
        } else {
          me.current = '-' + me.current
        }
      } else if (key === '<=') {
        me.current = me.current.substring(0, me.current.length - 1)
      } else if (key === '%') {
        me.current = (parseFloat(me.current) / 100).toString()
      } else if (key === 'π') {
        me.current = (parseFloat(me.current) * Math.PI).toString()
      } else if (key === 'x ²') {
        me.current = Math.pow(parseFloat(me.current), 2).toString()
      } else if (key === '√') {
        me.current = Math.sqrt(parseFloat(me.current)).toString()
      } else if (key === 'sin') {
        me.current = Math.sin(parseFloat(me.current)).toString()
      } else if (key === 'cos') {
        me.current = Math.cos(parseFloat(me.current)).toString()
      } else if (key === 'tan') {
        me.current = Math.tan(parseFloat(me.current)).toString()
      } else if (key === 'log') {
        me.current = Math.log10(parseFloat(me.current)).toString()
      } else if (key === 'ln') {
        me.current = Math.log(parseFloat(me.current)).toString()
      } else if (key === 'x^') {
        me.current += '^'
      } else if (key === 'x !') {
        let num = parseInt(me.current)
        if (num === 0) {
          me.current = '1'
        } else if (num < 0) {
          me.errorMessage = '无效的输入：负数没有阶乘'
          me.current = ''
        } else {
          let result = 1
          for (let i = 2; i <= num; i++) {
            result *= i
          }
          me.current = result.toString()
        }
      } else if (key === 'e') {
        me.current = Math.exp(parseFloat(me.current)).toString()
      } else if (key === 'rad') {
        me.current = (parseFloat(me.current) * (Math.PI / 180)).toString()
      } else if (key === '∘') {
        me.current = (parseFloat(me.current) * (180 / Math.PI)).toString()
      }
    },
    changeModeEvent() {
      this.changeMode = !this.changeMode
    },
    isOperator(char) {
      return ['+', '-', '*', '/', '^'].includes(char)
    },
    isValidExpression(expr) {
      // Check for invalid patterns like multiple operators
      if (/[+\-*/]{2,}/.test(expr)) {
        return false
      }
      // Check for balanced parentheses
      let parenthesesCount = 0
      for (let char of expr) {
        if (char === '(') parenthesesCount++
        if (char === ')') parenthesesCount--
        if (parenthesesCount < 0) return false
      }
      if (parenthesesCount !== 0) return false

      // Check if expression starts or ends with an operator
      if (this.isOperator(expr[0]) || this.isOperator(expr[expr.length - 1])) {
        return false
      }

      return true
    }
  }
}
</script>

<style scoped>
.calculator {
    width: 90vw;
    max-width: 440px;
    padding: 20px;
    border-radius: 10px;
    margin: 12vh auto;
    font-size: 16px;
    background-color: hsl(300, 7%, 38%);
}

.input {
    width: 95%;
    height: 50px;
    border-radius: 25px;
    border: 0;
    background-color: #333333;
    color: #d9d9d9;
    padding: 0 10px;
    margin: 10px 0;
    font-size: 30px;
}

.input:focus,
.input:active {
    border-color: #03a9f4;
    box-shadow: 0 0 4px #03A9F4;
    outline: none;
}

.button {
    margin: 5px;
    width: 63px;
    height: 30px;
    border: 0;
    border-radius: 15px;
    color: #000;
    font-size: 16px;
    background-color: #EEDAF6FF;
    cursor: pointer;
    outline: none;
}

.mode {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
}

.equal-sign {
    background-color: #6A9581FF;
    width: 133px;
}

.toggle-button {
    border: none;
    background-color: hsl(300, 7%, 38%);
    cursor: pointer;
    outline: none;
    font-size: 1rem;
    color: #fff;
}

.error-message {
    line-height: 30px;
    color: #ff0000;
    font-size: 14px;
    background-color: rgba(216, 214, 223, 0.59);
    margin-top: 5px;
    margin-bottom: 10px;
    text-align: center;
    height: 30px;
    border-radius: 15px;
}

@media (max-width: 480px) {
    .calculator {
        font-size: 3.4vw;
        padding: 4.3vw;
    }

    .input {
        height: 10.7vw;
        font-size: 6.4vw;
        border-radius: 5.3vw;
        padding: 0 2.1vw;
        margin: 2.1vw 0;
    }

    .button {
        width: 13.5vw;
        height: 6.4vw;
        margin: 1.1vw;
        border-radius: 3.2vw;
        font-size: 3.4vw;
    }

    .equal-sign {
        width: 28.5vw;
    }

    .toggle-button {
        font-size: 3.4vw;
    }

    .error-message {
        line-height: 6.4vw;
        font-size: 3vw;
        margin-top: 1.1vw;
        margin-bottom: 2.1vw;
        height: 6.4vw;
        border-radius: 3.2vw;
    }
}
</style>
