# vue-index

> 一个Vue的字母索引组件

## Build Setup

``` JavaScript
// 单文件组件，直接引用
import VueIndex from '@/components/VueIndex'

export default {
  components: {
    VueIndex
  },
  data () {
    return {
      indexDatas: [{
        character: 'A',
        items: [{
          name: 'fuck rm -rfA',
          selected: true
        }]
      }, {
        character: 'C',
        items: [{
          name: 'fuck rm -rfC',
          selected: false
        }]
      }, {
        character: 'D',
        items: [{
          name: 'fuck rm -rf D',
          selected: false
        }]
      }, {
        character: 'E',
        items: [{
          name: 'fuck rm -rfE',
          selected: false
        }]
      }]
    }
  },
  methods: {
    clickSubItem (subItem, subIndex, pre) {
      // pre为上一个selected为true的item
      // 为了方便改变selected状态
      pre.selected = false
      subItem.selected = true
    },
    clickItem (item, index) {
      console.log(item, index)
    }
  }
}
```
props有

- indexData(Array, requied): 数据
- trottleTime(Number): 节流时间
- isAllCharacters(Boolean): 在不是每个字母都有对应数据的情况下是否要显示所有的字母
- showEmpty(Boolean): 是否要把空数据的项显示出来
- crossPosition(String['top', 'middle']): 字母索引列表的位置

- @sub-item-click(subItem, subIndex, [preSelected]): 每个字母下的子项的item和index，preSelected是前一个selected的项，方便做selected切换
- @item-click(item, index): 每个字母的项目集合item和index

```html
<template>
  <div id="app">
    <!-- 需要一个外壳div包住 -->
    <div class="vue-index-wapper">
      <VueIndex
        :throttle-time="250"
        :index-datas="indexDatas"
        :is-all-characters="false"
        :show-empty="true"
        :cross-position="'middle'"
        @sub-item-click="clickSubItem"/>
    </div>
  </div>
</template>
```