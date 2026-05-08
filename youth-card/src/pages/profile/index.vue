<template>
  <view class="page">
    <view class="header">
      <view class="user-info">
        <view class="avatar">
          <text class="avatar-icon">👤</text>
        </view>
        <view class="user-text">
          <text class="nickname">青年用户</text>
          <view class="card-no">
            <text class="card-no-text">青年卡号：YQ20250001</text>
          </view>
          <text class="signature">努力奋斗，未来可期 ✨</text>
        </view>
      </view>
    </view>

    <view class="youth-card">
      <view class="card-deco-circle"></view>
      <view class="card-top">
        <text class="card-type">YOUTH CARD</text>
        <text class="card-label">青年卡</text>
      </view>
      <view class="card-body">
        <text class="card-number">YQ 2025 0001</text>
      </view>
      <view class="card-bottom">
        <view class="card-holder">
          <text class="card-holder-label">持卡人</text>
          <text class="card-holder-name">青年用户</text>
        </view>
        <view class="card-expire">
          <text class="card-expire-label">有效期</text>
          <text class="card-expire-date">2025-12</text>
        </view>
      </view>
    </view>

    <view class="stats-card">
      <view
        v-for="(stat, index) in statsList"
        :key="index"
        class="stat-item"
        hover-class="stat-item-active"
        @tap="onStatTap(stat)"
      >
        <text class="stat-num">{{ stat.num }}</text>
        <text class="stat-label">{{ stat.label }}</text>
      </view>
    </view>

    <view class="quick-entry">
      <view
        v-for="(entry, index) in quickEntries"
        :key="index"
        class="entry-item"
        hover-class="entry-item-active"
        @tap="navigateTo(entry.path)"
      >
        <view class="entry-icon-wrap">
          <text class="entry-icon">{{ entry.icon }}</text>
        </view>
        <text class="entry-name">{{ entry.name }}</text>
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
          <view class="menu-icon-wrap">
            <text class="menu-icon">{{ item.icon }}</text>
          </view>
          <text class="menu-text">{{ item.label }}</text>
        </view>
        <text class="menu-arrow">›</text>
      </view>
    </view>

    <view class="logout-btn" hover-class="logout-btn-active" @tap="onLogout">
      <text class="logout-text">退出登录</text>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const statsList = ref([
  { num: 12, label: '订单数', path: '/pages/profile/orders' },
  { num: 28, label: '收藏数', path: '/pages/profile/favorites' },
  { num: 6, label: '课程数', path: '/pages/profile/courses' }
])

const quickEntries = ref([
  { icon: '📦', name: '我的订单', path: '/pages/profile/orders' },
  { icon: '⭐', name: '我的收藏', path: '/pages/profile/favorites' },
  { icon: '📖', name: '我的课程', path: '/pages/profile/courses' },
  { icon: '🎫', name: '我的优惠券', path: '/pages/profile/orders' }
])

const menuList = ref([
  { icon: '📋', label: '志愿记录', path: '/pages/profile/orders' },
  { icon: '🏠', label: '租房管理', path: '/pages/profile/favorites' },
  { icon: '💼', label: '求职档案', path: '/pages/profile/courses' },
  { icon: '📝', label: '意见反馈', path: '/pages/profile/about' },
  { icon: 'ℹ️', label: '关于我们', path: '/pages/profile/about' },
  { icon: '⚙️', label: '设置', path: '/pages/profile/settings' }
])

const navigateTo = (url: string) => {
  uni.navigateTo({ url })
}

const onStatTap = (stat: { num: number; label: string; path: string }) => {
  uni.navigateTo({ url: stat.path })
}

const onLogout = () => {
  uni.showModal({
    title: '提示',
    content: '确定要退出登录吗？',
    success: (res) => {
      if (res.confirm) {
        uni.showToast({ title: '已退出登录', icon: 'none' })
      }
    }
  })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #faf8f5;
  padding-bottom: 40rpx;
}

.header {
  padding: 80rpx 40rpx 40rpx;
  background-color: #faf8f5;
}

.user-info {
  display: flex;
  align-items: center;
}

.avatar {
  width: 120rpx;
  height: 120rpx;
  border-radius: 50%;
  background: #f5f0ea;
  border: 3rpx solid #e8e0d6;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.avatar-icon {
  font-size: 60rpx;
}

.user-text {
  margin-left: 28rpx;
  display: flex;
  flex-direction: column;
}

.nickname {
  font-size: 36rpx;
  font-weight: 700;
  color: #1a1612;
}

.card-no {
  margin-top: 12rpx;
  background: #fef3ee;
  border-radius: 999rpx;
  padding: 6rpx 20rpx;
  align-self: flex-start;
}

.card-no-text {
  font-size: 22rpx;
  color: #c2410c;
}

.signature {
  font-size: 24rpx;
  color: #a89888;
  margin-top: 10rpx;
}

.youth-card {
  position: relative;
  margin: 24rpx 32rpx;
  padding: 36rpx 40rpx;
  background: #1a1612;
  border-radius: 16rpx;
  overflow: hidden;
}

.card-deco-circle {
  position: absolute;
  width: 120rpx;
  height: 120rpx;
  border-radius: 50%;
  background: #c2410c;
  opacity: 0.2;
  bottom: -30rpx;
  right: -20rpx;
}

.card-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
}

.card-label {
  font-size: 32rpx;
  font-weight: 700;
  color: #c2410c;
}

.card-type {
  font-size: 20rpx;
  color: rgba(255, 255, 255, 0.4);
  letter-spacing: 4rpx;
}

.card-body {
  margin: 36rpx 0 32rpx;
  position: relative;
}

.card-number {
  font-size: 36rpx;
  font-weight: 700;
  color: #ffffff;
  letter-spacing: 6rpx;
}

.card-bottom {
  display: flex;
  justify-content: space-between;
  position: relative;
}

.card-holder,
.card-expire {
  display: flex;
  flex-direction: column;
}

.card-holder-label,
.card-expire-label {
  font-size: 20rpx;
  color: rgba(255, 255, 255, 0.4);
}

.card-holder-name,
.card-expire-date {
  font-size: 26rpx;
  color: #ffffff;
  margin-top: 4rpx;
}

.stats-card {
  margin: 0 32rpx 24rpx;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  display: flex;
}

.stat-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 32rpx 0;
}

.stat-item-active {
  background: #f5f0ea;
  border-radius: 16rpx;
}

.stat-num {
  font-size: 36rpx;
  font-weight: 700;
  color: #1a1612;
}

.stat-label {
  font-size: 24rpx;
  color: #a89888;
  margin-top: 8rpx;
}

.quick-entry {
  margin: 0 32rpx 24rpx;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  display: flex;
  padding: 32rpx 0;
}

.entry-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.entry-item-active {
  opacity: 0.6;
}

.entry-icon-wrap {
  width: 88rpx;
  height: 88rpx;
  border-radius: 50%;
  background: #f5f0ea;
  display: flex;
  align-items: center;
  justify-content: center;
}

.entry-icon {
  font-size: 40rpx;
}

.entry-name {
  font-size: 24rpx;
  color: #6b5e52;
  margin-top: 12rpx;
}

.menu-card {
  margin: 0 32rpx 24rpx;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  overflow: hidden;
}

.menu-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100rpx;
  padding: 0 32rpx;
  border-bottom: 1rpx solid #f0ebe3;
}

.menu-item-last {
  border-bottom: none;
}

.menu-item-active {
  background: #f5f0ea;
}

.menu-left {
  display: flex;
  align-items: center;
}

.menu-icon-wrap {
  width: 56rpx;
  height: 56rpx;
  border-radius: 50%;
  background: #f5f0ea;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 20rpx;
}

.menu-icon {
  font-size: 30rpx;
}

.menu-text {
  font-size: 30rpx;
  color: #1a1612;
}

.menu-arrow {
  font-size: 36rpx;
  color: #e8e0d6;
}

.logout-btn {
  margin: 40rpx 32rpx 0;
  height: 88rpx;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logout-btn-active {
  opacity: 0.6;
  background: #f5f0ea;
}

.logout-text {
  font-size: 30rpx;
  color: #c2410c;
}
</style>
