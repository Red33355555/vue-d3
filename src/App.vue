<template>
  <header class="title">
    <h1>Data Charts</h1>
  </header>
  <section class="main">
    <base-card>
      <hirizontal-bar-chart
        v-if="collection && collection.length > 0"
        :collection="collection"
        identifier="horizontal-chart"
      ></hirizontal-bar-chart>
    </base-card>
    <base-card>
      <vertical-bar-chart
        v-if="collection && collection.length > 0"
        :collection="collection"
        identifier="vertical-chart"
      ></vertical-bar-chart>
    </base-card>
    <base-card>
      <line-chart
        v-if="lineGraphCollection && lineGraphCollection.length > 0"
        :collection="lineGraphCollection"
        identifier="line-chart-1"
      ></line-chart>
    </base-card>
    <base-card>
      <pie-chart
        v-if="collection && collection.length > 0"
        :collection="collection"
        identifier="pie-chart"
      ></pie-chart>
    </base-card>
  </section>
</template>

<script>
import BaseCard from './components/UI/BaseCard';
import HorizontalBarChart from './components/HorizontalBarChart';
import VerticalBarChart from './components/VerticalBarChart';
import LineChart from './components/LineChart';
import PieChart from './components/PieChart';
export default {
  components:{
    'hirizontal-bar-chart':HorizontalBarChart,
    'vertical-bar-chart':VerticalBarChart,
    'line-chart':LineChart,
    'pie-chart':PieChart,
    'base-card':BaseCard
  },
  data(){
      return{
        collection: [10,20,30,59,40],
        lineGraphCollection: [
          {
            x:20,
            y:20
          },
          {
            x:50,
            y:40
          },
          {
            x:60,
            y:50
          },
          {
            x:70,
            y:60
          },
          {
            x:80,
            y:70
          },
          {
            x:90,
            y:80
          },
          {
            x:90,
            y:90
          },
          {
            x:95,
            y:80
          },
          {
            x:95,
            y:75
          },
          {
            x:155,
            y:95
          },
        ],
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
  flex-wrap: wrap;
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
