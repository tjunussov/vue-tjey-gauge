<template lang="pug">
transition(name="gauge" :duration="5000" appear)
  div.tjey-gauge
    svg.gauge(style="width:100%; height:100%" :viewBox="'0 0 '+200+' '+200" x="0" y="0")
      circle.line-back(v-for="(v,i) in values" :r="getRadius(i)" cx="100" cy="100" :stroke="baseColor" :stroke-width="strokeWidth" fill="none")
      circle.line(v-for="(v,i) in values" :r="getRadius(i)" cx="100" cy="100" :style="{ 'stroke-dasharray':getArcDash((v.value * 360 ) / v.total,i)}" :stroke="colors[i]" :stroke-width="strokeWidth" fill="none")
    ul.legend
      li(v-for="(v,i) in values")
        div(v-if="v.total_label")
          .bullet(:style="{'backgroundColor':baseColor}") 
          slot(name="legend-item" :label="v.total_label" :value="v.total")
            | {{v.total_label}} {{v.total }}
        .bullet(:style="{'backgroundColor':colors[i]}") 
        slot(name="legend-item" :label="v.label" :value="v.value")
          | {{v.label}} {{v.value }}
</template>

<script>
export default {
  name: 'TJeyGauge',
  props: {
    colors:{default:()=>{return ['#642e85','#ff6413','#0084c0','#00b68e']}}, 
    baseColor:{default:'#eee9f1'},
    values:{type:Array, required:true},
    strokeWidth:{
      type:Number,
      default:4
    },
    spacing:{
      type:Number,
      default:30
    }
  },
  computed:{
    length(){
      return this.values.length
    },
  },
  data () {
    return {
      // values:[270,185,110,90]
    }
  },
  methods:{
    getRadius(i){
      return 80-(this.length - i)*15+this.spacing
    },
    getArc(degrees,radius){
      return (degrees  ) * (Math.PI / 180) * radius
    },
    getArcDash(degrees,i){
      var radius = this.getRadius(i);
      var arc = this.getArc(degrees,radius);
      return  arc + ',' + ( (2*Math.PI*radius) - arc )
    }
  }
}
</script>

<style lang="stylus">

.tjey-gauge
  text-align center
  
  circle
    transform scale(1,-1) rotate(90deg)
    transform-origin 50% 50%
    
    &.line
      stroke-dasharray 1 1
      transition stroke-dasharray 1s ease-out
      
  ul.legend
    list-style none
    display inline-block
    text-align left
    margin-left -2rem
    
    .bullet
      display inline-block
      width 0.7rem
      height 0.7rem
      margin-right 0.7rem
        
        
.gauge-enter
  opacity 0.5
  

  


  
  
  
    
</style>