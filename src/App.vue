<template>
  <div id="app">
    <div class="vue-index-wapper">
      <VueIndex :index-datas="indexDatas"/>
    </div>
  </div>
</template>

<script>
import VueIndex from '@/components/VueIndex'
export default {
  name: 'App',
  data () {
    return {
      indexDatas: [{
        character: 'A',
        items: [{
          name: 'fuck rm -rf'
        }]
      }]
    }
  },
  components: {
    VueIndex
  }
}
</script>

<style lang="scss" scoped>
body, document, #app {
  width: 100%;
  height: 100%;
  margin: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  .vue-index-wapper {
    width: 300px;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
  }
}
</style>
