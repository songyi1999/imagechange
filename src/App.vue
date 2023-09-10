<template>
  <div id="app">
   
    <HelloWorld :images="images" />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data(){
    return{
      images: (() => {
        let imagePaths = [];
        for (let i = 1; i <= 10; i++) {
          imagePaths.push(`images/${i}.jpg`);
        }
        return imagePaths;
      })(),
    }
  },
  
}
</script>

<style>

</style>
