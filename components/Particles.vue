<template>
          <vue-particles
          class="top"
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

}
</script>

<style scoped>
.top{
  margin-top: -50px;
max-height: auto;
}
</style>