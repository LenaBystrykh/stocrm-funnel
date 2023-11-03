<template>
  <div id="app">
    <FunnelPage />
  </div>
</template>

<script>
import FunnelPage from './components/FunnelPage.vue'

export default {
  name: 'App',
  components: {
    FunnelPage
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  padding: 20px;
  background-color: rgb(241, 241, 241);
  width: 100%;
  height: 100vh;
}
</style>
