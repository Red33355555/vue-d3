<template>
  <h1>D3 Examples</h1>
  <hirizontal-bar-chart
    v-if="collection && collection.length > 0"
    :collection="collection"
  ></hirizontal-bar-chart>
</template>

<script>
import HorizontalBarChart from './components/HoeizontalBarChart'
export default {
  components:{
    'hirizontal-bar-chart':HorizontalBarChart
  },
  data(){
      return{
        collection: [10,20,30,59,40],
        dataSet:{
          0:[10,20,30,59,40],
          1:[100,20,300,59,40],
          2:[10,200,30,59,40],
          3:[60,20,30,509,40],
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
</style>
