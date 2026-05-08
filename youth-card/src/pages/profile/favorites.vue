<template>
  <view class="page">
    <view class="container">
      <text class="icon">⭐</text>
      <text class="title">我的收藏</text>
      <text class="tip">功能开发中，敬请期待...</text>
    </view>
  </view>
</template>

<script setup lang="ts">
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #F5F6FA;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
}

.icon {
  font-size: 100rpx;
  margin-bottom: 32rpx;
}

.title {
  font-size: 36rpx;
  color: #333333;
  font-weight: 600;
  margin-bottom: 16rpx;
}

.tip {
  font-size: 28rpx;
  color: #999999;
}
</style>
