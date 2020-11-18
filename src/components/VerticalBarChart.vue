<template>
  <section>
    <div id="vertical-chart" class="bottom-border"></div>
    <div class="chart-name">
      <h1>Vertical Bar Chart</h1>
    </div>
  </section>
</template>

<script>
import * as d3 from 'd3';
export default {
    name:'vertical-bar-chart',
    props:{
        collection:{
            type:[],
            required: true
        },
        barColor:{
            type:String,
            required:false,
            default:'steelblue'
        },
        backgroundColor:{
            type:String,
            required:false,
            default:'white'
        }
    },
    data(){
        return{
            width: 500,
            height:500,
            scale:null,
            colorScale:null,
            yAxis:null,
            canvas:null,
            group: null
        };
    },
  mounted(){
    this.setScales();
    this.generateCanvas();
  },
  computed:{
    barPadding(){
    return this.width * 0.04;
    },
    chartInnerWidth(){
        return this.width - this.barPadding * 2;
    },
    chartInnerHeight(){
        return this.height - this.barPadding * 2;
    },
    barWidth(){
      return (this.chartInnerWidth / (this.collection.length * 2));
    }
  },
  methods:{
    generateCanvas(){
      d3.select('#vertical-chart').selectAll('svg').remove();
        const canvas = d3.select("#vertical-chart")
        .append("svg")
        .style('background-color',this.backgroundColor )
        .attr('width',this.width)
        .attr('height',this.height);
        const group = canvas.append('g')
        .attr('transform', 'translate('+this.barPadding+' '+this.barPadding+')');
        this.addBars(group);
        this.addAxis(group);
    },
    setScales(){
      this.scale = d3.scaleLinear()
      .domain([0,d3.max(this.collection)])
      .range([0,this.chartInnerHeight]);
      this.yAxis = d3.axisLeft()
      .scale(this.scale);
    },
    addBars(group){
      group
      .selectAll('rect')
      .data(this.collection)
      .join('rect')
      .attr('fill',this.barColor)
      .attr('height',d=> this.scale(d))
      .attr('width',this.barWidth - this.barPadding)
    //   .attr('y',d => (this.chartInnerHeight - this.scale(d)))
      .attr('transform', (d,i) =>'translate('+ (this.barWidth*i*2 + this.barPadding) + ' '+(this.chartInnerHeight - this.scale(d)) +')');
    },
    addAxis(group){
        group.append('g').call(this.yAxis)
        .attr('x', this.barPadding);
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

<style scoped>
section {
  padding: 10px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.bottom-border {
  padding-top: 5px;
  border-bottom-style: solid;
  border-bottom-width: 1px;
  border-bottom-color: #efefef;
}
.chart-name{
  padding-top: 5px;
}
</style>