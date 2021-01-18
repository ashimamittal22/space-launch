<template>
  <div id="app">
    <h2 style="text-align:left; width: 100%; margin-left:10px">SpaceX Launch Programs</h2>
    <div id="filter">
      <h3 style="text-align:left; margin-left:10px">Filter</h3>
      <div>
        <span id="launchYear">Launch Year</span>
      </div>
      <div style="display:inline-block">
        <div class="yearWrapper" v-for="(year, index) in years" :key="index">
          <button v-on:click="yearSelected" v-bind:class="{buttonClass, active: year === selectedYear}">{{year}}</button>
        </div>
      </div>
      <div >
        <span id="launchYear">Successful Launch</span>
      </div>
      <div >
        <div class="yearWrapper" v-for="(option, index) in launch" :key="index">
          <button v-on:click="selectLaunch" v-bind:class="{buttonClass, active: option === selectedLaunch}">{{option}}</button>
        </div>
      </div>
      <div >
        <span id="launchYear">Successful Landing</span>
      </div>
      <div >
        <div class="yearWrapper" v-for="(option, i) in landing" :key="i">
          <button v-on:click="selectLanding" v-bind:class="{buttonClass, active: option === selectedLand}">{{option}}</button>
        </div>
      </div>
    </div>
    <div id="container">
      <div v-for="(item, ind) in data" :key="ind">
        <space id="space" v-bind:item="item"></space>
      </div>
      
    </div>
  </div>
</template>

<script>
 import space from './components/space.vue'
 import axios from 'axios';

export default {
  computed: {
    data(){
      return this.infoData
    }
  },
  mounted () {
    axios
      .get('https://api.spacexdata.com/v3/launches?limit=100')
      .then(response => {
        console.log(response)
        this.info = response.data
        this.infoData = response.data
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  },
  data(){
    return{
      info : [],
      infoData: [],
      years: ["2006","2007","2008","2009","2010","2011","2012","2013","2014","2015","2016","2017","2018","2019","2020"],
      launch:["true", "false"],
      landing: ["true","false"],
      selectedYear: "",
      selectedLaunch: "",
      selectedLand: "",
      buttonClass: "buttonClass"
    }
  },
  name: 'App',
  components: {
    space
  },
  methods: {
    yearSelected(e){
      
      this.selectedYear = e.target.innerHTML
      this.filter(this.selectedYear, this.selectedLaunch, this.selectedLand)
    },
    selectLaunch(e){
      this.selectedLaunch = e.target.innerHTML
      this.filter(this.selectedYear, this.selectedLaunch, this.selectedLand)
    },
    selectLanding(e){
      this.selectedLand = e.target.innerHTML
      this.filter(this.selectedYear, this.selectedLaunch, this.selectedLand)
    },
    filter(year, launch, land){
      this.infoData = this.info
      if(year !== ""){
        this.infoData = this.infoData.filter(space => space.launch_year == year)
      }
      if(launch !== ""){
        this.infoData = this.infoData.filter(space => space.launch_success.toString() == launch)
      }
      if(land !== "" || land != null){
        this.infoData = this.infoData.filter(space => space.rocket.first_stage.cores[0].land_success.toString() == land)
      }
        
    }
  }
}
</script>

<style>
.yearWrapper{
  width: 50%;
  float: left;
}
#filter{
  background-color: white;
  border-radius: 8px;
}
#space{
  border-radius: 8px;
}
.buttonClass {
  border: none;
  color: white;
  padding: 8px 25px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  background-color: rgb(181, 236, 181);
  border-radius: 7px;
}
.buttonClass:focus{
  outline: none;
}
.active{
  background-color: green;
  border: none;
  border-radius: 7px;
  outline: none;
}
#launchYear{
    border-bottom: 1px solid;
    width: 80%;
    display: block;
    margin: auto;
    margin-bottom: 5px;
}
@media (max-width: 700px) {
        #filter{
          width: 90%;
          display: inline-block;
          margin-bottom: 15px;
        }
        #container{
          width: 90%;
          display: inline-block;
        }
        #space{
          width: 100%;
          background: white;
          margin-top: 15px;
          padding-top: 15px;
          text-align: left;
        }
}
@media (min-width: 700px) and (max-width: 1024px) {
        #filter{
          width: 20%;
          float: left;
        }
        #container{
          width: 80%;
          float: right;
        }
        #space{
          width: 50%;
          display: inline;
          float: left;
          background: white;
          text-align: left;
        }
}   
@media (min-width: 1024px){
       #filter{
          width: 20%;
          float: left;
          margin: 10px;
        }
        #container{
          margin: auto;
          width: 75%;
          float: right;
        }
        #space{
          width: 19%;
          height: 390px;
          display: inline;
          float: left;
          margin: 10px;
          background: white;
          padding: 10px;
          text-align: left;
        }
}    
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 10px;
  background:#f2f2f2;
  display: inline-block;
  width: 100%;
}
</style>
