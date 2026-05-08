<template>
  <view class="page">
    <view class="container">
      <text class="icon">💼</text>
      <text class="title">求职</text>
      <text class="desc">功能开发中，敬请期待...</text>
    </view>
  </view>
</template>

<script setup lang="ts">
</script>

<style>
.page {
  min-height: 100vh;
  background-color: #F5F6FA;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.icon {
  font-size: 80rpx;
  margin-bottom: 32rpx;
}

.title {
  font-size: 36rpx;
  font-weight: 700;
  color: #333333;
  margin-bottom: 20rpx;
}

.desc {
  font-size: 28rpx;
  color: #999999;
}
</style>
