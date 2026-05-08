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
      <view class="card-accent"></view>
      <view class="card-content">
        <view class="card-top">
          <view class="card-info-left">
            <text class="card-number-label">卡号</text>
            <text class="card-number">YQ 2025 0001</text>
          </view>
          <text class="card-label">青年卡</text>
        </view>
        <view class="card-bottom">
          <view class="card-field">
            <text class="card-field-label">持卡人</text>
            <text class="card-field-value">青年用户</text>
          </view>
          <view class="card-field">
            <text class="card-field-label">有效期</text>
            <text class="card-field-value">2025-12</text>
          </view>
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
  background-color: #F1F3F4;
  padding-bottom: 40rpx;
}

.header {
  padding: 80rpx 40rpx 40rpx;
  background-color: #F1F3F4;
}

.user-info {
  display: flex;
  align-items: center;
}

.avatar {
  width: 120rpx;
  height: 120rpx;
  border-radius: 50%;
  background: #E8F1F5;
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
  color: #1C1C1E;
}

.card-no {
  margin-top: 12rpx;
  background: rgba(65,108,129,0.08);
  border-radius: 999rpx;
  padding: 6rpx 20rpx;
  align-self: flex-start;
}

.card-no-text {
  font-size: 22rpx;
  color: #416C81;
}

.signature {
  font-size: 24rpx;
  color: #8E8E93;
  margin-top: 10rpx;
}

.youth-card {
  margin: 24rpx 32rpx;
  background: #FFFFFF;
  border-radius: 24rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
  overflow: hidden;
  display: flex;
}

.card-accent {
  width: 8rpx;
  background: linear-gradient(to bottom, #416C81, #2D5A6F);
  border-radius: 4rpx;
  flex-shrink: 0;
}

.card-content {
  flex: 1;
  padding: 32rpx 36rpx;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.card-top {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
}

.card-info-left {
  display: flex;
  flex-direction: column;
}

.card-number-label {
  font-size: 22rpx;
  color: #8E8E93;
  margin-bottom: 8rpx;
}

.card-number {
  font-size: 34rpx;
  font-weight: 700;
  color: #1C1C1E;
  letter-spacing: 4rpx;
}

.card-label {
  font-size: 30rpx;
  font-weight: 700;
  color: #416C81;
}

.card-bottom {
  display: flex;
  margin-top: 32rpx;
  gap: 64rpx;
}

.card-field {
  display: flex;
  flex-direction: column;
}

.card-field-label {
  font-size: 22rpx;
  color: #8E8E93;
}

.card-field-value {
  font-size: 26rpx;
  color: #1C1C1E;
  margin-top: 4rpx;
}

.stats-card {
  margin: 0 32rpx 24rpx;
  background: #FFFFFF;
  border-radius: 16rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
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
  background: #F8F9FA;
  border-radius: 16rpx;
}

.stat-num {
  font-size: 36rpx;
  font-weight: 700;
  color: #1C1C1E;
}

.stat-label {
  font-size: 24rpx;
  color: #8E8E93;
  margin-top: 8rpx;
}

.quick-entry {
  margin: 0 32rpx 24rpx;
  background: #FFFFFF;
  border-radius: 16rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
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
  background: rgba(65,108,129,0.08);
  display: flex;
  align-items: center;
  justify-content: center;
}

.entry-icon {
  font-size: 40rpx;
}

.entry-name {
  font-size: 24rpx;
  color: #8E8E93;
  margin-top: 12rpx;
}

.menu-card {
  margin: 0 32rpx 24rpx;
  background: #FFFFFF;
  border-radius: 16rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
  overflow: hidden;
}

.menu-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100rpx;
  padding: 0 32rpx;
  border-bottom: 0.5rpx solid #E5E5EA;
}

.menu-item-last {
  border-bottom: none;
}

.menu-item-active {
  background: #F8F9FA;
}

.menu-left {
  display: flex;
  align-items: center;
}

.menu-icon-wrap {
  width: 56rpx;
  height: 56rpx;
  border-radius: 50%;
  background: rgba(65,108,129,0.08);
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
  color: #1C1C1E;
}

.menu-arrow {
  font-size: 36rpx;
  color: #C6C6C8;
}

.logout-btn {
  margin: 40rpx 32rpx 0;
  height: 88rpx;
  background: #FFFFFF;
  border-radius: 16rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
  display: flex;
  align-items: center;
  justify-content: center;
}

.logout-btn-active {
  background: #F8F9FA;
}

.logout-text {
  font-size: 30rpx;
  color: #FF3B30;
}
</style>
