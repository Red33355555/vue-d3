<template>
  <section>
    <div :id="identifier" class="bottom-border"></div>
    <div class="chart-name">
      <h1>Horizontal Bar Chart</h1>
    </div>
  </section>
</template>

<script>
import * as d3 from 'd3';
export default {
    name:'horizontal-bar-chart',
    props:{
        collection:{
            type:Array[Number],
            required: true
        },
        identifier:{
          type:String,
          required:true
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
      xAxis:null,
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
        return this.height * 0.04;
    },
    chartInnerWidth(){
        return this.width - this.barPadding * 2;
    },
    chartInnerHeight(){
        return this.height - this.barPadding * 2;
    },
    barHeight(){
      return (this.chartInnerHeight / (this.collection.length * 2));
    }
  },
  methods:{
    generateCanvas(){
      d3.select(`#${this.identifier}`).selectAll('svg').remove();
        const canvas = d3.select(`#${this.identifier}`)
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
      .range([0,this.chartInnerWidth]);
      this.xAxis = d3.axisBottom()
      .scale(this.scale);
    },
    addBars(group){
      group
      .selectAll('rect')
      .data(this.collection)
      .join('rect')
      .attr('fill',this.barColor)
      .attr('height',this.barHeight)
      .attr('width',d=>this.scale(d))
      .attr('transform', (d,i) =>'translate('+[0+' '+(this.barHeight * 2 * i)]+')');
    },
    addAxis(group){
        group.append('g').call(this.xAxis)
        .attr('transform', 'translate(0 '+(this.height - (this.barPadding * 2))+')');
        group.append("text")
        .attr("x", this.chartInnerHeight - (this.barPadding * 2))
        .attr("y", this.chartInnerWidth)
        .attr("fill", "currentColor")
        .text('X-Axis');
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
  border-bottom-style: solid;
  border-bottom-width: 1px;
  border-bottom-color: #efefef;
}
.chart-name {
  padding-top: 5px;
}
</style>
