<template>
  <view class="container">

    <uni-nav-bar>
      <template v-slot:default>
        <view class="record-text-wrap">
          <text class="record-text active">支出</text>
          <text class="record-text">收入</text>
        </view>
      </template>
      <template v-slot:right>
        <text @click="F.quit">取消</text>
      </template>
    </uni-nav-bar>

    <!--  <uni-grid :column="6" :highlight="true">-->
    <!--    <uni-grid-item v-for="(tag, index) in tags" :index="index" :key="index">-->
    <!--      <view class="grid-item-box">-->
    <!--        <uni-tag :text="tag.name" :circle="true" type="primary"></uni-tag>-->
    <!--      </view>-->
    <!--    </uni-grid-item>-->
    <!--  </uni-grid>-->

    <view class="tag-wrap">
      <view v-for="(tag, i) of tags" :class="F.CLASS.tagItemAt(i)" @click="F.toggleTagAt(i)">{{ tag.name }}</view>
    </view>

    <view class="keyboard-wrap">
      <view>{{ record.text }}</view>
      <table>
        <tr v-for="(row, i) of _.chunk(operators, 4)">
          <td v-for="(item, j) of row" @click="F.clickTdAt(i*4 + j)">{{ item.name }}</td>
        </tr>
      </table>
    </view>
  </view>
</template>

<script setup lang="ts">
import _ from 'lodash'
import {ref} from 'vue'

const TAGS = [
  {name: '餐饮'},
  {name: '购物'},
  {name: '日用'},
  {name: '交通'},

  {name: '蔬菜'},
  {name: '水果'},
  {name: '零食'},
  {name: '运动'},

  {name: '娱乐'},
  {name: '通讯'},
  {name: '服饰'},
  {name: '美容'},

  {name: '住房'},
  {name: '居家'},
  {name: '孩子'},
  {name: '长辈'},
]
const OPERATORS = [
  {name: 7},
  {name: 8},
  {name: 9},
  {name: '今天'},
  {name: 4},
  {name: 5},
  {name: 6},
  {name: '前1天'},
  {name: 1},
  {name: 2},
  {name: 3},
  {name: '后1天'},
  {name: '.'},
  {name: 0},
  {name: '删除'},
  {name: '完成'},
]
const RECORD = {
  value: 0,  // 记账金额
  text: 0,  // 记账金额文本
  date: new Date().toISOString().slice(0, 10),  // 记录日期
  tags: [],  // 标签
  note: '',  // 备注
}
const RECORDS: Array<any> = []

const PROCESS = {
  tags(tags: Array<any>) {
    return tags.map(x => {
      x._isChosen = false
      x._type = 'default'
      return x
    });
  },
  operators(operators: Array<any>) {
    return operators
  },
  record(r: any) {
    r._isDotClicked = false
    r._multiplier = 10
    r._text = '0'

    return r
  },
}

const tags = ref(PROCESS.tags(TAGS))
const operators = ref(PROCESS.tags(OPERATORS))
const record = ref(PROCESS.record(RECORD))
const records = ref(RECORDS)

const toggleTag = (tag: any) => {
  if (tag && tag._isChosen!==undefined) {
    tag._isChosen = !tag._isChosen
  }
}

const formatRecordText = (record: any) => {
  console.log(record.value)
  record.value.text = _.trimStart(record.value.text, '0')
  if (record.value.text==='') record.value.text = '0'

  console.log(record.value)
}

const F = {
  CLASS: {
    tagItemAt(i: number) {
      const classes = ['tag-item']
      tags.value[i]._isChosen && classes.push('active')
      return classes.join(' ')
    }
  },

  quit() {
    console.log('123')
  },

  toggleTagAt(i: number) {
    return toggleTag(tags.value[i]);
  },

  clickTdAt(i: number) {
    const name = operators.value[i].name
    // if (0 <= name && name <= 9) {
    //   if (record.value._isDotClicked) {
    //     record.value._multiplier /= 10
    //   }
    //   record.value.value = record.value.value * record.value._multiplier + name * record.value._multiplier / 10
    //
    // } else if (name==='.') {
    //   record.value._isDotClicked = true
    //
    //   record.value.value += 0.0
    // }
    let text: string = `${record.value.text}`
    if (name==='删除') {
      text = text.substring(0, text.length - 1)
    } else if (name==='今天') {

    } else if (name==='前1天') {

    } else if (name==='后1天') {

    } else if (name==='完成') {

    } else {
      text = `${text}${name}`
      // 去除冗余前缀0
      if (!text.startsWith('0.')) text = _.trimStart(text, '0')

      // 只保留2位小数
      let index: number = text.indexOf('.')
      if (-1!==index) text = text.substring(0, index + 3)
    }

    if (text==='') text = '0'

    let value = Number(text)
    if (!isNaN(value)) {
      record.value.text = text
      record.value.value = value
    }

  },
}

</script>

<style lang="scss">
.container {
  font-size: 32rpx;

  .record-text-wrap {
    display: flex;
    flex: 1;
    justify-content: center;

    .record-text {
      align-self: center;
      margin: 0 20rpx;
      padding: 10rpx 0;

      &.active {
        border-bottom-style: solid;
        border-bottom-color: red;
      }
    }
  }

  $tag-wrap-margin: 15rpx;
  $tag-margin: 5rpx;
  $tag-n: 6;
  $tag-width: calc(calc(750rpx - 2 * $tag-wrap-margin) / $tag-n - 2 * $tag-margin);

  .tag-wrap {
    margin: 0 $tag-wrap-margin;
    display: flex;
    flex-flow: row wrap;

    .tag-item {
      margin: $tag-margin;

      background-color: #F6F6F6;
      //color: #5C5249;
      border-radius: 50%;
      width: $tag-width;
      height: $tag-width;
      line-height: $tag-width;
      text-align: center;
      align-self: center;

      &.active {
        background-color: #F7D955;
      }
    }
  }

  .keyboard-wrap {
    width: 100%;
    //height: 300rpx;
    //background-color: red;
    position: absolute;
    bottom: 0;
    //box-shadow: #007aff;

    table {
      margin: 0 5rpx;
      width: 100%;
      text-align: center;

      tr {
        height: 100rpx;
      }

      td {
        border: 1px solid #F1F1F1;
        width: 185rpx;
        //background-color: #007aff;
      }
    }
  }
}
</style>
