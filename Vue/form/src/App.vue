<template>
  <MyHome/>
  <GameSimon/>
</template>

<script>
import MyHome from './components/MyHome.vue'
import GameSimon from "@/components/GameSimon.vue";

export default {
  name: 'App',
  components: {
    GameSimon,
    MyHome
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
form input{
  padding: 5px;
  outline: none;
}
</style>
