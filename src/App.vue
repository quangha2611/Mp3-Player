<template>
  <div id="app">
    <Player id="player"/>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import Player from './components/Player.vue'
export default {
  name: 'App',
  components: {
    //HelloWorld
    Player
  }
}
</script>

<style scoped>
*{
	margin: 0 auto;
	padding: 0;
}
</style>
