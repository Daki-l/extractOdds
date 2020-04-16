<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
    name: 'App',
    created() {
        if (document.documentElement.clientWidth > 640) {
            require('./style/index.css');
            this.plaform = 'pc';
        } else {
            require('./style/mobile.css');
            this.plaform = 'mobile';
        }
    }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
