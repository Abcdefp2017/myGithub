<template>
  <div class="calculator-border">
    <div class="input">
      <div class="formula" v-cloak>{{current}}</div>
      <div class="result" v-cloak>{{result}}</div>
    </div>
    <div class="row1">
      <el-button type="primary" round @click="press" ><span class="buttonModify">AC</span></el-button>
      <el-button type="primary" round @click="press"><span class="buttonModify">+/-</span></el-button>
      <el-button type="primary" round @click="press">%</el-button>
      <el-button type="primary" round @click="press">/</el-button>
    </div>
    <div class="row2">
      <el-button type="primary" round @click="press">7</el-button>
      <el-button type="primary" round @click="press">8</el-button>
      <el-button type="primary" round @click="press">9</el-button>
      <el-button type="primary" round @click="press"><span class="mulModify">*</span></el-button>
    </div>
    <div class="row3">
      <el-button type="primary" round @click="press">4</el-button>
      <el-button type="primary" round @click="press">5</el-button>
      <el-button type="primary" round @click="press">6</el-button>
      <el-button type="primary" round @click="press">-</el-button>
    </div>
    <div class="row4">
    <el-button type="primary" round @click="press">1</el-button>
    <el-button type="primary" round @click="press">2</el-button>
    <el-button type="primary" round @click="press">3</el-button>
    <el-button type="primary" round @click="press">+</el-button>
    </div>
    <div class="row5">
    <el-button type="primary" round @click="press">0</el-button>
    <el-button type="primary" round @click="press">.</el-button>
    <el-button type="primary" round @click="press"><span class="buttonModify">{{back}}</span></el-button>
    <el-button type="primary" round @click="press">=</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
      current: '',
      result: 0,
      number1: '',
      operator: '',
      number2: '',
      back: '<='
    }
  },
  methods: {
    press: function (event) {
      // 计算机按钮click事件
      let self = this
      let key = event.target.textContent
      switch (key) {
        case '+':
        case '-':
        case '*':
        case '/':
          arithmetic(key)
          self.current += key
          break
        case '%':
          percentage()
          break
        case 'AC':
          clear()
          break
        case '+/-':
          changeSymbol()
          break
        case '<=':
          back(key)
          break
        case '=':
          result(key)
          break
        default:
          getNumber(key)
      };

      // 获取数字
      function getNumber (number) {
        self.current += key
        console.log(self.operator)
        if (self.operator === '' || self.operator === '=') {
          self.number1 = parseFloat(self.number1.toString() + number)
        } else {
          self.number2 = parseFloat(self.number2.toString() + number)
        }
      };
      // 清空输入框
      function clear () {
        self.current = ''
        self.result = 0
        self.number1 = ''
        self.number2 = ''
        self.operator = ''
      };

      // 符号取反
      function changeSymbol () {
        self.current = -self.current
        self.number1 = self.current
      };

      // 除以100
      function percentage () {
        self.current = parseFloat(Number(self.current / 100).toFixed(10))
        self.number1 = self.current
      };

      // 回退一格
      function back () {
        let str = self.current.toString()
        self.current = str.substring(0, str.length - 1)
        let lastChar = str[str.length - 1]
        switch (lastChar) {
          case '+':
          case '-':
          case '*':
          case '/':
            self.operator = ''
            break
          default:
            if (self.number2 === '') {
              self.number1 = parseFloat(self.current)
            } else {
              let numberAll = self.current.match(/\d+/g)
              if (numberAll.length === 1) {
                self.number2 = ''
              } else {
                self.number2 = parseFloat(numberAll[numberAll.length - 1])
              }
            }
        };
        self.current = str.substring(0, str.length - 1)
      };

      // 输出结果
      function result (operator) {
        arithmetic(operator)
        console.log(self.operator)
        self.result = self.number1
        if (self.result !== 0) {
          self.current = self.result
          self.number1 = self.result
        } else {
          self.current = ''
          self.number1 = ''
        }
      };

      // 四则运算
      function arithmetic (operator) {
        if (self.number2 === '' || self.operator === '=') {
          self.operator = operator
        } else {
          let equals
          switch (self.operator) {
            case '+':
              equals = parseFloat(self.number1 + self.number2)
              break
            case '-':
              equals = parseFloat(self.number1 - self.number2)
              break
            case '*':
              equals = parseFloat(self.number1 * self.number2)
              break
            case '/':
              if (self.number2 === 0) {
                alert('除数不能为0')
                equals = 0
              } else {
                equals = parseFloat(self.number1 / self.number2)
              }
              break
          }
          self.number1 = equals
          self.current = self.number1
          self.number2 = ''
          self.operator = operator
        }
      };
    }
  }
}
</script>

<style>
/*Calculator*/
.input{
    border: 0.15rem solid #b9b4b4;
    background-color: #f5f8f8;
    margin: 1rem;
    height: 5rem;
    width: 24rem;
    font-size: 2rem;
    border-radius: 0.4rem;
    box-shadow: 0.1rem 0.1rem 0.5rem #b55858;
    text-align: right;
    left: 145px;
    top: 12px;
    position: relative;
}

.el-button{
  border: 0.1rem solid rgba(166, 98, 98, 0.74);
  width: 5rem;
  height: 4rem;
  display: inline-block;
  text-align: center;
  line-height: 2rem;
  margin-left: 0px;
  margin-right: 0.5rem;
  cursor: pointer;
  border-radius: 0.5rem;:
  background-color: rgba(244, 135, 135, 0.78);
  box-shadow: 0.1rem 0.1rem 0.3rem rgb(85, 65, 65);
  transition: all 0.1s;
  font-size: 40px;
  color: #ffffff;
}
.el-button--primary:hover{
  background: green;
}
.buttonModify{
  right: 10px;
  position: relative;
}
.mulModify{
  top: 10px;
  position: relative;
}
</style>
