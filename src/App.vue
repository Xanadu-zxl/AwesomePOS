<template>
  <div id="app">
    <leftNav />
    <div class="main">
      <router-view />
    </div>
  </div>
</template>

<script>
import leftNav from '@/components/common/leftNav'
export default {
  name: 'App',
  components: {
    leftNav
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.main {
  overflow: hidden;
  float: left;
  background: #eff2f7;
  height: 100%;
  width: 95%;
}
</style>
