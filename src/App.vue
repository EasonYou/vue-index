<template>
  <div id="app">
    <div class="vue-index-wapper">
      <VueIndex
        :index-datas="indexDatas"
        :is-all-characters="false"
        :show-empty="false"
        @sub-item-click="clickSubItem"/>
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
          name: 'fuck rm -rf',
          selected: true
        }]
      }, {
        character: 'C',
        items: [{
          name: 'fuck rm -rf'
        }]
      }]
    }
  },
  methods: {
    clickSubItem (subItem, subIndex) {
      console.log(subItem, subIndex)
    },
    clickItem (item, index) {
      console.log(item, index)
    }
  },
  components: {
    VueIndex
  }
}
// , {
//         character: 'D',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'E',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'F',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'G',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'H',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'I',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'J',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'K',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'L',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'M',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'O',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'P',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }, {
//         character: 'Q',
//         items: [{
//           name: 'fuck rm -rf'
//         }]
//       }
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
