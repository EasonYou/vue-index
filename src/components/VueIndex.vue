
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
          background: #e0e0e0;
          text-align: left;
          h3 {
            padding-left: 20px;
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
              padding-left: 20px;
            }
            &:active {
              background: #f7f7f7;
            }
          }
          .list-item-checked {
            background: #f1f1f1;
          }
        }
        .no-item {
          height: 50px;
          text-align: center;
          line-height: 50px;
          &:active {
            background: #ffe4e4;
          }
        }
      }
    }
  }
  .side-index-wapper {
    width: 20px;
    height: 100%;
    position: absolute;
    right: 10%;
    top: 50%;
    transform: translateY(-50%);
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
        // border-left: 1px solid #efefef;
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
      text-align: center;
      transform: translate(-50%, -50%);
    }
  }
}
</style>

<template>
  <div
    class="vue-index" ref="main">
    <div
      class="index-lists-wapper"
      ref="scrollTarget">
      <div
        class="index-lists-content"
        ref="scrollWapper">
        <div
          v-for="(item, index) in finalDatas"
          :data-character="item.character"
          :key="index"
          class="list-item">
            <div
              @click="bindItemClick(item, index)"
              class="item-title">
              <h3>{{item.character}}</h3>
            </div>
            <div
              v-if="item.items.length !== 0"
              class="sub-lists-wapper">
              <div
                v-for="(subItem, subIndex) in item.items"
                :key="subIndex"
                @click="bindSubItemClick(subItem, subIndex)"
                class="sub-list-item"
                :class="{'list-item-checked': subItem.selected}">
                <span>{{subItem.name}}</span>
              </div>
            </div>
            <div
              class="no-item"
              v-if="!item.items.length">
              没有内容
            </div>
          </div>
          <div
            class="no-contents"
            v-if="!finalDatas.length">
            没有内容
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
        v-for="(item, index) in finalCharacters"
        :key="index"
        class="side-index-item"
        :style="{height: middlePositionHeight}">
        <div
          class="side-item-name"
          :style="{'right': isMouseDown ? '10%' : ''}">
          <span>{{item}}</span>
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
      characters: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
      middlePositionHeight: ''
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
    },
    showEmpty: {
      type: Boolean,
      default: true
    },
    isAllCharacters: {
      type: Boolean,
      default: true
    },
    crossPosition: {
      type: String,
      default: 'top'
    }
  },
  computed: {
    finalDatas () {
      if (this.showEmpty) {
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
      return this.indexDatas
    },
    finalCharacters () {
      if (this.isAllCharacters || this.indexDatas.length === 26 || this.showEmpty) {
        return this.characters
      }
      const arr = []
      this.indexDatas.forEach((item) => {
        arr.push(item.character)
      })
      return arr
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
      return this.finalCharacters[index]
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
    },
    bindSubItemClick (subItem, subIndex) {
      this.$emit('sub-item-click', subItem, subIndex, this.preSelected)
      this.preSelected = subItem
    },
    bindItemClick (item, index) {
      this.$emit('item-click', item, index)
    },
    crossMiddleBinding () {
      if (this.crossPosition === 'middle') {
        const main = this.$refs.main
        const height = getComputedStyle(main, null)['height']
        const len = this.finalCharacters.length
        let middleHeight = len * 38 / 1000 * parseInt(height)
        this.$refs.sideIndexWapper.style.height = `${middleHeight}px`
        this.middlePositionHeight = `${100 / len}%`
      }
    },
    findPreItem (flag) {
      const self = this
      this.indexDatas.forEach((data) => {
        data.items.some((item) => {
          if (item.selected === true) {
            self.preSelected = item
            return true
          }
        })
      })
    }
  },
  watch: {
    character (newVal, oldVal) {
      const scrollTarget = this.$refs.scrollTarget
      const scrollWapper = this.$refs.scrollWapper
      const scrollItems = Array.from(scrollWapper.childNodes)
      let offsetTop
      scrollItems.some((item) => {
        if (item.className === 'list-item' && item.dataset.character === newVal) {
          offsetTop = item.offsetTop
          return true
        }
      })
      scrollTarget.scrollTop = offsetTop === undefined ? scrollTarget.scrollTop : offsetTop
    }
  },
  created () {
    const self = this
    this.findPreItem()
    document.addEventListener('mouseup', () => {
      self.isMouseDown = false
      self.modalFlag = false
    })
  },
  mounted () {
    this.crossMiddleBinding()
  },
  beforeUpdate () {
    this.findPreItem()
  },
  updated () {
    this.crossMiddleBinding()
  },
  beforeDestroy () {
    document.removeEventListener('mouseup', () => {})
  }
}
</script>
