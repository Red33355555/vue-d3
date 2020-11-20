<template>
  <section>
    <div :id="identifier" class="bottom-border"></div>
    <div class="chart-name">
      <h1>Line Graph</h1>
    </div>
  </section>
</template>

<script>
import * as d3 from 'd3';
export default {
    name:'vertical-bar-chart',
    props:{
        collection:{
            type:Array[{x:Number,y:Number}],
            required: true
        },
        identifier:{
          type:String,
          required:true
        },
        width:{
            type:Number,
            required:true
        },
        height:{
            type:Number,
            required:true
        },
        lineColor:{
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
            xScale:null,
            yScale:null,
            colorScale:null,
            xAxis:null,
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
            d3.select(`#${this.identifier}`).selectAll('svg').remove();
            const canvas = d3.select(`#${this.identifier}`)
                .append("svg")
                .style('background-color',this.backgroundColor )
                .attr('width',this.width)
                .attr('height',this.height);
            const group = canvas.append('g')
                .attr('transform', 'translate('+this.barPadding+' '+this.barPadding+')');
            this.plotLine(group);
            this.addAxis(group);
        },
        setScales(){
            this.yScale = d3.scaleLinear()
                .domain([0,d3.max(this.collection, d => d.y)])
                .range([this.chartInnerHeight,0]); 
            this.xScale = d3.scaleLinear()
                .domain([0,d3.max(this.collection, d => d.x)])
                .range([0,this.chartInnerHeight]);
            this.xAxis = d3.axisBottom()
                .scale(this.xScale);
            this.yAxis = d3.axisLeft()
                .scale(this.yScale);
        },
        plotLine(group){
            // const line = d3.line(this.collection);
            group.append('path')
            .datum(this.collection)
            .attr('fill','none')
            .attr("stroke", "steelblue")
            .attr("stroke-width", 1.5)
            .attr('d',d3.line(this.collection)
            .x(d => this.xScale(d.x))
            .y(d => this.yScale(d.y)));
        },
        addAxis(group){
            group.append('g').call(this.yAxis)
                .attr('x', this.barPadding);
            group.append("text")
                .attr("x", - this.barPadding)
                .attr("y", 0)
                .attr("fill", "currentColor")
                .text('Y-Axis');
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
        this.setScales();
        this.generateCanvas();
      },
      width(){
        this.setScales();
        this.generateCanvas();
      },
      height(){
        this.setScales();
        this.generateCanvas();
      },
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
.chart-name {
  padding-top: 5px;
}
</style>