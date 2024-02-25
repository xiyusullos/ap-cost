<template>
  <!--  <view class="content">-->
  <uni-list>
    <uni-list-item right-text="-20">
      <template v-slot:header>
        <uni-tag :circle="true" text="标签"/>
        <uni-tag :circle="true" text="标签"/>
      </template>
    </uni-list-item>
    <uni-list-item title="娱乐" right-text="-100"></uni-list-item>
  </uni-list>


  <!--  </view>-->

</template>

<script setup lang="ts">
import {ref} from "vue";

const title = ref("Hello");
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
</style>
