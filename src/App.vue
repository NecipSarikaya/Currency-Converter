<template>
    <div class="container">
      <div class="row">
        <div class="col-md-6">
        <br>
        <input type="text" class="form-control" v-model="base">
        <br>
        <select class="form-control" v-model="baseCountry" @change="getData">
          <option v-for="country in countryList" >{{country}}</option>
        </select>
        </div>
      </div>
      <div class="row">
        <div class="col-md-6">
          <hr>
          <br>
          <select class="form-control" v-model="convertCountry" @change="baseValueFinder">
            <option v-for="country in countryList">{{country}}</option>
          </select>
          <br>
          <button class="btn btn-success" @click="convertMoney">Convert</button>
          <br>
          <br>
          <label for="" class="form-control">{{converted}}</label>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      countryList : [],
      valueList:[],
      base : null,
      baseValue:null,
      baseCountry:null,
      converted : null,
      convertCountry:null
    }
  },
  methods:{
    getData(index){
      this.countryList =[]
      this.valueList = []
    fetch('https://api.exchangeratesapi.io/latest?base='+ (this.baseCountry = (this.baseCountry === null) ? 'USD' : this.baseCountry))
    .then(response => response.json())
    .then(data =>{
      const propertyNames = Object.keys(data.rates);
      const propertyValues = Object.values(data.rates);
      for(let key = 0 ; key < propertyNames.length;key++){
        this.countryList.push(propertyNames[key])
      }
      for(let key = 0 ; key < propertyValues.length;key++){
        this.valueList.push(propertyValues[key])
      }
    }).catch(() =>{
    })
    },
    convertMoney(){
      this.converted = this.base * this.baseValue
    },
    baseValueFinder(){
      let index = 0
      index = this.countryList.indexOf(this.convertCountry)
      this.baseValue = this.valueList[index];
    }
  },
  created() {
    this.getData();
  },
 
}
</script>

<style>

</style>
