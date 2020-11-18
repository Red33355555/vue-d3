<template>
  <header class="title">
    <h1>Data Charts</h1>
  </header>
  <section class="main">
    <base-card>
      <hirizontal-bar-chart
        v-if="collection && collection.length > 0"
        :collection="collection"
      ></hirizontal-bar-chart>
    </base-card>
    <base-card>
      <vertical-bar-chart
        v-if="collection && collection.length > 0"
        :collection="collection"
      ></vertical-bar-chart>
    </base-card>
  </section>
</template>

<script>
import BaseCard from './components/UI/BaseCard'
import HorizontalBarChart from './components/HoeizontalBarChart'
import VerticakBarChart from './components/VerticalBarChart'
export default {
  components:{
    'hirizontal-bar-chart':HorizontalBarChart,
    'vertical-bar-chart':VerticakBarChart,
    'base-card':BaseCard
  },
  data(){
      return{
        collection: [10,20,30,59,40],
        dataSet:{
          0:[10,20,30,59,40],
          1:[100,20,300,59,40,77],
          2:[10,200,30,90,55,59,40],
          3:[60,20,30,99.99,250,88,93,509,40],
          4:[80,70,30,59,40]
        },
        count:0
      };
    },
    mounted(){
      this.alterData();
    },
    methods:{
      alterData(){
      setInterval(()=>{
        if(this.count<5){
          this.collection = this.dataSet[this.count];
          this.count++;
          }else clearInterval();
      },3000)
    }
    }
  }
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif, Arial, Helvetica;
}
.main {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
.title {
  padding: 5px;
  margin-bottom: 10px;
  display: flex;
  justify-content: center;
  background-color: white;
  border-bottom-style: solid;
  border-bottom-width: 1px;
  border-bottom-color: #efefef;
}
</style>
