
<style lang="scss" scoped>
.vue-index {
  height: 100%;
  width: 100%;
  overflow-y: hidden;
  position: relative;
  user-select:none;
  .index-lists-wapper {
    height: 100%;
    overflow-y: scroll;
    .index-lists-content {
      .list-item {
        .item-title {
          height: 60px;
          background: #efefef;
          text-align: left;
          h3 {
            line-height: 60px;
            margin: 0;
          }
        }
        .sub-lists-wapper {
          .sub-list-item {
            height: 50px;
            border-bottom: 1px solid #efefef;
            text-align: left;
            span {
              line-height: 50px;
            }
          }
        }
      }
    }
  }
  .side-index-wapper {
    width: 20px;
    height: 100%;
    position: absolute;
    right: 25px;
    top: 10px;
    // display: flex;
    // flex-wrap: wrap;
    .side-index-item {
      height: 3.8%;
      width: 100%;
      position: relative;
      .side-item-name  {
        display: block;
        width: 20px;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        right: 0;
        border-left: 1px solid #efefef;
      }
      cursor: pointer;
    }
    .modal {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 50%;
      height: 100px;
      line-height: 100px;
      font-size: 35px;
      background: rgba(0, 0, 0, 0.4);
      color: #fff;
      border-radius: 10px;
      transform: translate(-50%, -50%);
    }
  }
}
.no-scroll-bar {

}
</style>

<template>
  <div
    class="vue-index">
    <div
      class="index-lists-wapper"
      ref="scrollTarget">
      <div
        class="index-lists-content"
        ref="scrollWapper">
        <div
          class="list-item"
          v-for="(item, index) in finalDatas"
          :data-character="item.character"
          :key="index">
            <div
              class="item-title">
              <h3>{{item.character}}</h3>
            </div>
            <div
              v-if="item.items.length !== 0"
              class="sub-lists-wapper">
              <div
                class="sub-list-item"
                v-for="(subItem, subIndex) in item.items"
                :key="subIndex">
                <span>{{subItem.name}}</span>
              </div>
            </div>
            <div
              class="no-item"
              v-if="!item.items.length">
              没有内容
            </div>
          </div>
      </div>
    </div>
    <div
      class="side-index-wapper"
      :style="{'width': isMouseDown ? '100%' : '', 'right': isMouseDown ? '0' : ''}"
      ref="sideIndexWapper"
      @mousedown="bindSideItemMouseDownEvent"
      @mousemove="bindSideItemMouseMoveEvent"
      @mouseup="bindSideItemMouseUpEvent">
      <div
        class="modal"
        v-if="modalFlag">
        {{character}}
      </div>
      <div
        v-for="(item, index) in finalDatas"
        :key="index"
        class="side-index-item">
        <div
          class="side-item-name"
          :style="{'right': isMouseDown ? '25px' : ''}">
          <span>{{item.character}}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'vue-index',
  data () {
    return {
      isMouseDown: false,
      modalFlag: false,
      character: '',
      characters: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']
    }
  },
  props: {
    indexDatas: {
      type: Array,
      required: true
    },
    throttleTime: {
      type: Number,
      default: 250
    }
  },
  computed: {
    finalDatas () {
      let finalDatas = []
      this.characters.forEach((character) => {
        let hasCharacter = false
        this.indexDatas.some((data) => {
          if (data.character === character) {
            hasCharacter = true
            finalDatas.push(data)
          }
        })
        if (!hasCharacter) {
          finalDatas.push({
            character,
            items: []
          })
        }
      })
      return finalDatas
    }
  },
  methods: {
    isCanTrigger (e) {
      let target = e.target
      let flag = false
      while (target) {
        if (target && target.className === 'side-index-item') {
          flag = true
          break
        }
        target = target.parentNode
      }
      if (!flag) {
        return false
      }
      let index = Array.from(this.$refs.sideIndexWapper.childNodes).indexOf(target) - 2
      return this.characters[index]
    },
    switchCharacter (e) {
      if (this.isMouseDown) {
        let character = this.isCanTrigger(e)
        if (character && character !== this.character) {
          this.character = character
          if (!this.modalFlag) {
            this.modalFlag = true
          }
        }
      }
    },
    bindSideItemMouseDownEvent (e) {
      this.isMouseDown = true
      this.lastTime = new Date()
      this.switchCharacter(e)
    },
    bindSideItemMouseMoveEvent (e) {
      const nowTime = new Date()
      if (nowTime - this.lastTime > this.throttleTime) {
        this.lastTime = nowTime
        this.switchCharacter(e)
      }
    },
    bindSideItemMouseUpEvent (e) {
      this.isMouseDown = false
      this.modalFlag = false
    }
  },
  watch: {
    character (newVal, oldVal) {
      console.log(newVal)
      const scrollTarget = this.$refs.scrollTarget
      const scrollWapper = this.$refs.scrollWapper
      const scrollItems = Array.from(scrollWapper.childNodes)
      let offsetTop
      scrollItems.some((item) => {
        if (item.dataset.character === newVal) {
          offsetTop = item.offsetTop
          return true
        }
      })
      scrollTarget.scrollTop = offsetTop
      // console.log(scrollItems)
    }
  },
  created () {
    const self = this
    // this.characters = []
    // this.indexDatas.forEach((item) => {
    //   this.characters.push(item.character)
    // })
    document.addEventListener('mouseup', () => {
      console.log('hehe')
      self.isMouseDown = false
      self.modalFlag = false
    })
  },
  beforeDestroy () {
    document.removeEventListener('mouseup', () => {})
  }
}
</script>
