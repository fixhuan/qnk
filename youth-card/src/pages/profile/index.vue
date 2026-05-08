<template>
  <view class="page">
    <view class="header">
      <view class="user-info">
        <view class="avatar">
          <text class="avatar-icon">👤</text>
        </view>
        <view class="user-text">
          <text class="nickname">点击登录</text>
          <text class="desc">青年卡，让生活更精彩</text>
        </view>
      </view>
    </view>

    <view class="menu-card">
      <view
        v-for="(item, index) in menuList"
        :key="index"
        class="menu-item"
        :class="{ 'menu-item-last': index === menuList.length - 1 }"
        hover-class="menu-item-active"
        @tap="navigateTo(item.path)"
      >
        <view class="menu-left">
          <text class="menu-icon">{{ item.icon }}</text>
          <text class="menu-text">{{ item.label }}</text>
        </view>
        <text class="menu-arrow">›</text>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { reactive } from 'vue'

interface MenuItem {
  icon: string
  label: string
  path: string
}

const menuList = reactive<MenuItem[]>([
  { icon: '📦', label: '我的订单', path: '/pages/profile/orders' },
  { icon: '⭐', label: '我的收藏', path: '/pages/profile/favorites' },
  { icon: '📖', label: '我的课程', path: '/pages/profile/courses' },
  { icon: 'ℹ️', label: '关于我们', path: '/pages/profile/about' },
  { icon: '⚙️', label: '设置', path: '/pages/profile/settings' }
])

const navigateTo = (url: string) => {
  uni.navigateTo({ url })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #F5F6FA;
}

.header {
  background: linear-gradient(180deg, #4F6EF7, #7B93FA);
  padding: 60rpx 40rpx 80rpx;
  border-radius: 0 0 40rpx 40rpx;
}

.user-info {
  display: flex;
  align-items: center;
}

.avatar {
  width: 80rpx;
  height: 80rpx;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.avatar-icon {
  font-size: 40rpx;
}

.user-text {
  margin-left: 24rpx;
  display: flex;
  flex-direction: column;
}

.nickname {
  font-size: 32rpx;
  color: #ffffff;
  font-weight: 600;
}

.desc {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.7);
  margin-top: 8rpx;
}

.menu-card {
  margin: 24rpx;
  background-color: #ffffff;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
  overflow: hidden;
}

.menu-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100rpx;
  padding: 0 32rpx;
  border-bottom: 1rpx solid #F0F0F5;
}

.menu-item-last {
  border-bottom: none;
}

.menu-item-active {
  background-color: #F5F6FA;
}

.menu-left {
  display: flex;
  align-items: center;
}

.menu-icon {
  font-size: 40rpx;
  margin-right: 20rpx;
}

.menu-text {
  font-size: 30rpx;
  color: #333333;
}

.menu-arrow {
  font-size: 36rpx;
  color: #CCCCCC;
}
</style>
