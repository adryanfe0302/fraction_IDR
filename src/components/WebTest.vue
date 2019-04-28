<template>
<div class="container">
  <div class="master">
    <span for="name"><b>IDR Amount</b></span>
    <input v-model="valueAmount" type="text" placeholder="Enter Name" name="name" required>
     <button type="submit" class="registerbtn" @click="onShowFraction()">Show Fraction</button>
  </div>
  <br /> <br />
  <div class="mainres">
    <div class="divloop bghead">
      <div class="div10">No</div>
      <div class="div30">Amount</div>
      <div class="div60">Fractions</div>
      <div class="div5"></div>
    </div>
    <div v-for='(list, index) in listAmount' :key="index" class="divloop">
      <div class="div10"> {{index + 1}} </div> 
      <div class="div30"> Rp {{formatNumber(list.currentValue)}} </div> 
      <div class="div60">
        <div class="subdiv" v-for='(val,index) in list.calculateValue' :key="index">
          <span> {{val[1]}}x </span>  
          <span><b>Rp {{formatNumber(val[0])}}</b> </span> 
        </div>
      </div>
      <div class="div5 fright cursor" @click="deletex(index)">&times;</div><br>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'Tokped-Test-Code',
  data () {
    return {
      fractionList: ['50', '100', '500', '1000', '2000', '5000','10000', '20000', '50000', '100000'],
      listAmount: [],
      valueAmount: '',
      resfilterone: '',
      id: '',
      GlobalFilter: '',
      invalidAmount: []
    }
  },
  methods: {
    onShowFraction () {
      if (this.valueAmount == '') {
        alert('Plese fill he Amount First!')
      } else if (this.valueAmount <= 49) {
        let obj = {
          currentValue: this.valueAmount,
          id: this.listAmount.length + 1,
          invalidAmount: this.invalidAmount
        }
        obj.calculateValue = [{
          '0': this.valueAmount + ' (No Available Fraction)'
        }]
        this.listAmount.push(obj)
        console.log('enter', this.listAmount)
      } else {
        this.filterone(this.fractionList, this.valueAmount)
        let filterVar = this.filterone(this.fractionList, this.valueAmount)
        this.GlobalFilter = filterVar
        this.resfilterone = filterVar[filterVar.length - 1]
        let obj = {
          currentValue: this.valueAmount,
          calculateValue: [this.resfilterone],
          id: this.listAmount.length + 1,
          invalidAmount: this.invalidAmount
        }
        this.listAmount.push(obj)
        this.filterAmount()
      }
      this.valueAmount = ''
    },
    filterAmount () {
      let valueArr = this.fractionList[0]
      for(var x = 0; x < valueArr; x++) {
        if (this.calcTrans != 0) {
          this.constFraction(this.listAmount.length)
        }
      }
      this.listAmount.forEach(x => {
        x.calculateValue.forEach((xx,ii) => {
          if (xx == undefined) {
            x.calculateValue.splice(ii)
          }
        })
        let counts = {}
        x.calculateValue.forEach(x => {
          counts[x] = (counts[x] || 0)+1;
        })
        console.log('count', counts)
        x.calculateValue = Object.entries(counts)
      })
    },
    constFraction (id) {
      this.filterone(this.GlobalFilter, this.calcTrans)
      let filterVar = this.filterone(this.GlobalFilter, this.calcTrans)
      this.valueAmount = this.calcTrans
      this.resfilterone = filterVar[filterVar.length - 1] 
      this.listAmount.forEach(x => {
        if (x.id == id) {
          x.calculateValue.push(this.resfilterone)
        }
      })
      // console.log('sa', this.calcTrans)
      // if (this.calcTrans == undefined) {
      //   console.log('true')
      // } else {
      //   console.log('false')
      // }
      // let getSmallest = []
      // if (this.calcTrans == undefined) {
      //   console.log('nothing')
      // } else {
      //   this.getSmallest.push(this.calcTrans)
      // }
      // console.log('gai', getSmallest)
      return this.resfilterone
    },
    filterone (arr,val) {
      return arr.filter(x => parseInt(val) >= parseInt(x))
    },
    deletex (e) { 
      this.listAmount.forEach(x => {
        if (x.id == e) {
          this.listAmount.splice(e, 1)
        } 
      })
    },
    formatNumber (num) {
      return parseInt(num).toFixed(2).replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,')
    }
  },
  computed: {
    calcTrans () {
      return this.valueAmount - this.resfilterone
    }
    // filterVar (arr, val) {
    //   return this.filterone(arr, val)
    // }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container{
  width: 500px;
}
.master{
  padding: 20px;
  background-color:white;
  width: 100%;
  border:1px solid #ddd;
  box-shadow: 0px 0px 5px #ddd;
  border-radius: 5px;
  display: inline-block;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
input[type=text]{
    width: calc(100% - 30px);
    padding: 10px;
    margin: 5px 0 10px 0;
    display: inline-block;
    border: none;
    background: #f1f1f1;
    font-size: 16px;
    font-family: cairo;
    border-radius: 2px;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
}

input[type=text]:focus{
    background-color: #ddd;
    outline: none;
}

.registerbtn {
    background-color: #4CAF50;
    color: white;
    padding: 10px 20px;
    margin: 8px 0;
    border: none;
    cursor: pointer;
    width: 100%;
    opacity: 0.9;
    font-size: 16px;
    font-family: inherit;
    text-transform: uppercase;
    letter-spacing: 2px;
}

.registerbtn:hover {
    opacity: 1;
}
.inline{
  display: inline;
}
.fright{
  float: right;
}
.divloop{
  padding: 20px;
  width: 100%;
  border-bottom: 1px solid #ddd;
  display: flex;
  text-align: left;
}
.div10{
  width: 10%;
}
.div5{
  width: 5%;
}
.div20{
  width: 20%;
}
.div30{
  width: 30%;
}
.div40{
  width: 40%;
}
.div60{
  width: 60%;
}
.subdiv{
  margin-bottom: 5px;
}
.cursor{
  cursor: pointer;
}
.bghead{
  background-color: whitesmoke;
}
</style>

// let fractionList = ['100', '500', '1000', '2000', '5000','10000', '20000', '50000', '100000']
// let curVal = 36500
// let finalAmount = []
// let filterone = fractionList.filter(x => parseInt(curVal) >= parseInt(x))
// let resfilterone = filterone[filterone.length - 1]
// finalAmount.push(resfilterone)

// let calcTrans = curVal - resfilterone
// let filtertwo = fractionList.filter(x => parseInt(calcTrans) >= parseInt(x))
// let resfiltertwo = filtertwo[filtertwo.length - 1]
// finalAmount.push(resfiltertwo)

// console.log(finalAmount)