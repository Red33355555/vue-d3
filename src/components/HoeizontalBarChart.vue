<template>
  <h3><strong>Data: </strong>{{ collection }}</h3>
  <div id="chart"></div>
</template>

<script>
import * as d3 from 'd3';
export default {
    name:'horizontal-bar-chart',
    props:['collection'],
  data(){
    return{
      width: 500,
      height:500,
      barPadding:10,
      scale:null,
      colorScale:null,
      canvas:null,
      group: null
    };
  },
  mounted(){
    this.setScales();
    this.generateCanvas();
    console.log('bin',d3.bin(this.collection));
  },
  computed:{
    data(){
      return this.collection;
    },
    barHeight(){
      return (this.height / this.collection.length - (this.barPadding * 2));
    },
    barWidthMultiplyer(){
      return (this.width -(this.barPadding * 2))/d3.max(this.collection);
    }
  },
  methods:{
    generateCanvas(){
      d3.select('#chart').selectAll('svg').remove();
    const canvas = d3.select("#chart")
      .append("svg")
      .style('background-color','red' )
      .attr('width',this.width)
      .attr('height',this.height);
    const group = canvas.append('g')
    .attr('transform', 'translate('+this.barPadding+' '+this.barPadding+')');
    this.addBars(group);
    },
    setScales(){
      this.scale = d3.scaleLinear()
      .domain([0,d3.max(this.collection)])
      .range([0,this.width - (this.barPadding *2)]);
    },
    addBars(group){
      group
      .selectAll('rect')
      .data(this.collection)
      .join('rect')
      .attr('fill','blue')
      .attr('width',d=>this.scale(d))
      .attr('height',this.barHeight)
      .attr('transform', (d,i) =>'translate(0 '+(100 * i)+')');
    }
  },
  watch:{
      collection(){
        console.log('data changed');
        this.setScales();
        this.generateCanvas();
      }
    }
}
</script>

<style>
</style>
