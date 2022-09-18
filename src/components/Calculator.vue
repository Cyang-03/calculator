<template>
  <div id="box">
    <table>
      <!-- 显示计算结果 -->
      <tr class="content">
        <td colspan="4">
          <span v-if="isShow">{{ this.num1 }} {{ this.code }} {{ this.num2 }}</span>
          <span v-else>{{ this.result }}</span>
        </td>
      </tr>
      <!-- 按钮 -->
      <tr>
        <td><input type="button" value="%" class="color" @click="getCode" /></td>
        <td><input type="button" value="CE" class="color" @click="clear"/></td>
        <td><input type="button" value="C" class="color" @click="clear"/></td>
        <td><input type="button" value="Del" class="color" @click="Del"/></td>
      </tr>
      <tr>
        <td><input type="button" value="1/x" class="color" @click="getCode"/></td>
        <td><input type="button" value="^2" class="color" @click="getCode"/></td>
        <td><input type="button" value="√" class="color" @click="getCode"/></td>
        <td><input type="button" value="/" class="color" @click="getCode"/></td>
      </tr>
      <tr>
        <td><input type="button" value='7' @click="getNum"/></td>
        <td><input type="button" value='8' @click="getNum"/></td>
        <td><input type="button" value='9' @click="getNum"/></td>
        <td><input type="button" value="*" class="color" @click="getCode"/></td>
      </tr>
      <tr>
        <td><input type="button" value="4" @click="getNum"/></td>
        <td><input type="button" value="5" @click="getNum"/></td>
        <td><input type="button" value="6" @click="getNum"/></td>
        <td><input type="button" value="-" class="color" @click="getCode"/></td>
      </tr>
      <tr>
        <td><input type="button" value="1" @click="getNum"/></td>
        <td><input type="button" value="2" @click="getNum"/></td>
        <td><input type="button" value="3" @click="getNum"/></td>
        <td>
          <input type="button" value="+" class="color" @click="getCode"/>
        </td>
      </tr>
      <tr>
        <td><input type="button" value="+/-" class="color" @click="change"/></td>
        <td><input type="button" value="0" @click="getNum"/></td>
        <td>
          <input type="button" value="." class="color" @click="getNum" id="btn"/>
        </td>
        <td>
          <input
            type="button"
            value="="
            style="background-color: rgb(51, 119, 132)"
            @click="equal"
          />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
// 解决js计算精度问题
import Demical from 'decimal.js'
let btn = document.getElementById('btn')
export default {
  data() {
    return {
      num1: '', // 第一个数
      code: '', // 运算符
      num2: '', // 第二个数
      isShow: true, // 显示数据
      before: true, // 前提触发条件
      result: '', // 运算结果
    };
  },
  methods: {
    getNum(e) { // 输入数字
      if (this.before === true) { // 一开始条件为true输入第一个数
        this.num1 += e.target.value
        if(this.num1[this.num1.length-1] === '.') { // 判断数字是否已经是小数，是则将小数按钮禁用
          document.getElementById('btn').disabled = true
        }
      } else { // 输入第二个数
        this.num2 += e.target.value 
        if(this.num2[this.num2.length-1] === '.') { 
          document.getElementById('btn').disabled = true
        }
      }
    },
    getCode(e) { // 输入运算符
      this.before = false
      // 点击运算符开启小数按钮
      document.getElementById('btn').disabled = false
      // 获取运算符给code变量
      this.code = e.target.value
      // 点击%, ^2,  √, 1/x进行运算
      if(e.target.value === '%' && this.num1 != '') {
        this.result = Demical.div(this.num1, 100)
        this.before = true
        this.num1 = this.result
        this.num1 = String(this.num1)
        this.code = ''
        this.num2 = ''
        this.isShow = true
        for(let i = 0; i < this.num1.length - 1; i++) {
          if(this.num1[i] === '.') {
            document.getElementById('btn').disabled = true
          }
        }
      }
      if(e.target.value === '^2' && this.num1 != '') {
        this.result = Demical.pow(this.num1, 2) 
        this.before = true
        this.num1 = this.result
        this.num1 = String(this.num1)
        this.code = ''
        this.num2 = ''
        this.isShow = true
        for(let i = 0; i < this.num1.length - 1; i++) {
          if(this.num1[i] === '.') {
            document.getElementById('btn').disabled = true
          }
        }
      }
      if(e.target.value === "√" && this.num1 != '') {
        this.result = Demical.sqrt(this.num1) 
        this.before = true
        this.num1 = this.result
        this.num1 = String(this.num1)
        this.code = ''
        this.num2 = ''
        this.isShow = true
        for(let i = 0; i < this.num1.length - 1; i++) {
          if(this.num1[i] === '.') {
            document.getElementById('btn').disabled = true
          }
        }
      }
      if(e.target.value === "1/x" && this.num1 != '') {
        this.result = Demical.div(1, this.num1) 
        this.before = true
        this.num1 = this.result
        this.num1 = String(this.num1)
        this.code = ''
        this.num2 = ''
        this.isShow = true
        for(let i = 0; i < this.num1.length - 1; i++) {
          if(this.num1[i] === '.') {
            document.getElementById('btn').disabled = true
          }
        }
      }
    },
    change() { // +/- 给1，2数字 加正负号
      if(+this.num1 > 0 && this.num2 === '') {
        this.num1 = -this.num1
        this.num1 = String(this.num1)
      } 
      else if(+this.num1 <= 0 && this.num2 === '') {
        this.num1 = -this.num1
        this.num1 = String(this.num1)
      }
      else if(+this.num2 > 0) {
        this.num2 = -this.num2
        this.num1 = String(this.num1)
      } else {
        this.num2 = -this.num2
        this.num1 = String(this.num1)
      }
    },
    equal() { //等于
    if( this.num2 != '' && this.code != '') {
      switch (this.code) { // 判断运算符输出运算结果
        case '+':
        this.result = Demical.add(this.num1, this.num2)
        break;
        case '-':
        this.result = Demical.sub(this.num1, this.num2)
        break;
        case '*':
        this.result = Demical.mul(this.num1, this.num2)
        break;
        case '/':
        this.result = Demical.div(this.num1, this.num2)
        break;
    }
        this.num1 = this.result
        this.num1 = String(this.num1)
        for(let i = 0; i < this.num1.length - 1; i++) {
          if(this.num1[i] === '.') {
            document.getElementById('btn').disabled = true
          }
        }
    } 
        this.before = true
        this.code = ''
        this.num2 = ''
        this.isShow = true

    },
    Del() { // 删除
        if( this.num1 != '' && this.code === '' && this.num2 === '' ) {
          this.num1 = this.num1.substr(0, this.num1.length - 1)
        } 
        else if( this.num1 != '' && this.code != '' && this.num2 === '' ) {
          this.code = this.code.substr(0, this.code.length - 1)
          this.before = true
        }
        else if( this.num1 != '' && this.code != '' && this.num2 != '' ) {
          this.num2 = this.num2.substr(0, this.num2.length - 1)
        }

    },
    clear() { // 清空
        this.num1 = '',
        this.code = '',
        this.num2 = '',
        this.before = true,
        this.isShow = true
        this.result = ''
        document.getElementById('btn').disabled = false
    }
  }
}
</script>

<style scoped>
table {
  margin: 53px auto;
  padding: 5px;
  border: 1px solid rgb(210, 208, 208);
  border-radius: 10px;
  background-color: rgb(249, 249, 249);
  overflow: hidden;
}
input {
  width: 100px;
  height: 80px;
  font-size: 18px;
  background-color: #fff;
  border: 1px solid rgb(201, 199, 199);
  border-radius: 10px;
}
.content {
  position: relative;
  height: 150px;
}
.content span {
  position: absolute;
  right: 0;
  bottom: 10px;
  display: block;
  width: 100%;
  padding-right: 10px;
  text-align: right;
  font-size: 50px;
}
.color {
  background-color: rgba(248, 246, 246, 0.3);
}
</style>