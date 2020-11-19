<template>
  <section>
    <div :id="identifier" class="bottom-border"></div>
    <div class="chart-name">
      <h1>Pie Chart</h1>
    </div>
  </section>
</template>
<script>
import * as d3 from 'd3';
export default {
    name:'pie-chart',
    props:{
        collection:{
            type:Array[Number],
            required: true
        },
        identifier:{
          type:String,
          required:true
        },
        backgroundColor:{
            type:String,
            required:false,
            default:'white',
            canvas:null
        }
    },
    data(){
        return{
            width: 500,
            height:500,
            colorScale:null,
            pie:null,
            readyData:null
        }
    },
    computed:{
        padding(){
        return this.width * 0.04;
        },
        chartInnerWidth(){
            return this.width - this.barPadding * 2;
        },
        chartInnerHeight(){
            return this.height - this.barPadding * 2;
        },
        radius(){
            return Math.min(this.width, this.height) / 2 - this.padding;
        }
    },
    mounted(){
        this.generateCanvas();
        this.addPath();
    },
    methods:{
        generateCanvas(){
            d3.select(`#${this.identifier}`).selectAll('svg').remove();
            this.colorScale = d3.scaleLinear()
            .domain([0,d3.max(this.collection)])
            .range(['red','blue']);
            this.canvas = d3.select(`#${this.identifier}`)
            .append('svg')
            .attr('width',this.width)
            .attr('height',this.height)
            .append('g')
            .attr('transform', `translate(${this.width/2},${this.height/2})`);
            this.pie = d3.pie().value(d => d);
            this.readyData = this.pie(this.collection);
            console.log('readyData',this.readyData);
        },
        addPath(){
            this.canvas.selectAll(`#${this.identifier}`)
            .data(this.readyData)
            .enter()
            .append('path')
            .attr('d',d3.arc().innerRadius(0).outerRadius(this.radius))
            .attr('fill',d => {console.log(d.data,this.colorScale(d.data)); return this.colorScale(d.data);})
            .attr('stroke','black')
            .style('stroke-width','2px')
            .style('opacity', 0.7);
        }
    },
    watch:{
        collection(){
            this.generateCanvas();
            this.addPath();
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