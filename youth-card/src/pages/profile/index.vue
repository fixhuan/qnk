<template>
  <view class="page">
    <view class="header">
      <view class="header-bg">
        <view class="wave"></view>
      </view>
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
          <text class="menu-icon">{{ item.icon }}</text>
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
  background-color: #F5F6FA;
  padding-bottom: 40rpx;
}

.header {
  position: relative;
  padding: 60rpx 40rpx 100rpx;
  overflow: hidden;
}

.header-bg {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(180deg, #4F6EF7, #7B93FA);
}

.wave {
  position: absolute;
  bottom: -2rpx;
  left: -10%;
  right: -10%;
  height: 60rpx;
  background-color: #F5F6FA;
  border-radius: 50% 50% 0 0;
}

.user-info {
  position: relative;
  display: flex;
  align-items: center;
}

.avatar {
  width: 120rpx;
  height: 120rpx;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.3);
  border: 4rpx solid #FFFFFF;
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
  color: #FFFFFF;
  font-weight: 700;
}

.card-no {
  margin-top: 10rpx;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 20rpx;
  padding: 4rpx 16rpx;
  align-self: flex-start;
}

.card-no-text {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.9);
}

.signature {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.7);
  margin-top: 10rpx;
}

.stats-card {
  margin: -50rpx 24rpx 24rpx;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.08);
  display: flex;
  position: relative;
  z-index: 1;
}

.stat-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 32rpx 0;
}

.stat-item-active {
  background-color: #F5F6FA;
  border-radius: 20rpx;
}

.stat-num {
  font-size: 40rpx;
  font-weight: 700;
  color: #333333;
}

.stat-label {
  font-size: 24rpx;
  color: #999999;
  margin-top: 8rpx;
}

.quick-entry {
  margin: 0 24rpx 24rpx;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
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
  opacity: 0.7;
}

.entry-icon-wrap {
  width: 88rpx;
  height: 88rpx;
  border-radius: 50%;
  background: linear-gradient(135deg, #EEF1FF, #DDE4FF);
  display: flex;
  align-items: center;
  justify-content: center;
}

.entry-icon {
  font-size: 40rpx;
}

.entry-name {
  font-size: 24rpx;
  color: #333333;
  margin-top: 12rpx;
}

.menu-card {
  margin: 0 24rpx 24rpx;
  background-color: #FFFFFF;
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

.logout-btn {
  margin: 40rpx 24rpx 0;
  height: 88rpx;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
}

.logout-btn-active {
  background-color: #FFF0F0;
}

.logout-text {
  font-size: 30rpx;
  color: #FF4D4F;
}
</style>
