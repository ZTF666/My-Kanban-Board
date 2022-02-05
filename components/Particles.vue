<template>
          <vue-particles
            :class="opacity"
            color="#0afc2e"
            :particleOpacity="0.7"
            :particlesNumber="80"
            shapeType="circle"
            :particleSize="10"
            linesColor="#0afc2e"
            :linesWidth="1"
            :lineLinked="true"
            :lineOpacity="0.4"
            :linesDistance="150"
            :moveSpeed="3"
            :hoverEffect="true"
            hoverMode="grab"
            :clickEffect="true"
            clickMode="push"
          >
        </vue-particles>
        
</template>

<script>
export default {
  data(){
    return{
      opacity:'tsparticles'
    }
  },
  mounted(){
    const op= localStorage.getItem('opacity')
    if(op){
    this.opacity=op
    }
  },
  created(){
    this.$nuxt.$on('toggleOpacity',()=>{
    if(this.opacity=='tsparticles'){
      localStorage.setItem('opacity','tsparticlesOp')
      this.opacity='tsparticlesOp'
    }else{
      localStorage.setItem('opacity','tsparticles')
      this.opacity='tsparticles'
    }
  })
  }
}
</script>
<style>
/* visible particles */
.tsparticles {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 0;
}
/* invisible particles */
.tsparticlesOp {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: -1;
}
</style>