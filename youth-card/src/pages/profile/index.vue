<template>
  <view class="page">
    <view class="header">
      <view class="header-orb header-orb-1"></view>
      <view class="header-orb header-orb-2"></view>
      <view class="header-orb header-orb-3"></view>
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
      <view class="card-orb card-orb-1"></view>
      <view class="card-orb card-orb-2"></view>
      <view class="card-deco-line"></view>
      <view class="card-top">
        <text class="card-label">青年卡</text>
        <text class="card-type">YOUTH CARD</text>
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
        <view class="entry-icon-wrap" :style="{ background: entry.gradient }">
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
          <view class="menu-icon-wrap" :style="{ background: item.gradient }">
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
  { icon: '📦', name: '我的订单', path: '/pages/profile/orders', gradient: 'linear-gradient(135deg, #667eea, #764ba2)' },
  { icon: '⭐', name: '我的收藏', path: '/pages/profile/favorites', gradient: 'linear-gradient(135deg, #f093fb, #f5576c)' },
  { icon: '📖', name: '我的课程', path: '/pages/profile/courses', gradient: 'linear-gradient(135deg, #4facfe, #00f2fe)' },
  { icon: '🎫', name: '我的优惠券', path: '/pages/profile/orders', gradient: 'linear-gradient(135deg, #43e97b, #38f9d7)' }
])

const menuList = ref([
  { icon: '📋', label: '志愿记录', path: '/pages/profile/orders', gradient: 'linear-gradient(135deg, #667eea, #764ba2)' },
  { icon: '🏠', label: '租房管理', path: '/pages/profile/favorites', gradient: 'linear-gradient(135deg, #4facfe, #00f2fe)' },
  { icon: '💼', label: '求职档案', path: '/pages/profile/courses', gradient: 'linear-gradient(135deg, #f093fb, #f5576c)' },
  { icon: '📝', label: '意见反馈', path: '/pages/profile/about', gradient: 'linear-gradient(135deg, #43e97b, #38f9d7)' },
  { icon: 'ℹ️', label: '关于我们', path: '/pages/profile/about', gradient: 'linear-gradient(135deg, #fa709a, #fee140)' },
  { icon: '⚙️', label: '设置', path: '/pages/profile/settings', gradient: 'linear-gradient(135deg, #a18cd1, #fbc2eb)' }
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
  background-color: #0f0f2d;
  padding-bottom: 40rpx;
}

.header {
  position: relative;
  padding: 80rpx 40rpx 40rpx;
  overflow: hidden;
}

.header-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80rpx);
}

.header-orb-1 {
  width: 400rpx;
  height: 400rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  opacity: 0.25;
  top: -200rpx;
  left: -100rpx;
}

.header-orb-2 {
  width: 300rpx;
  height: 300rpx;
  background: linear-gradient(135deg, #f093fb, #f5576c);
  opacity: 0.15;
  top: -80rpx;
  right: -60rpx;
}

.header-orb-3 {
  width: 200rpx;
  height: 200rpx;
  background: linear-gradient(135deg, #4facfe, #00f2fe);
  opacity: 0.1;
  bottom: -40rpx;
  left: 200rpx;
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
  background: rgba(15, 15, 45, 0.8);
  border: 4rpx solid transparent;
  background-clip: padding-box;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  box-shadow: 0 0 0 4rpx #667eea, 0 0 0 8rpx #764ba2, 0 0 30rpx rgba(124, 92, 252, 0.3);
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
  font-size: 38rpx;
  color: #FFFFFF;
  font-weight: 700;
}

.card-no {
  margin-top: 12rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-radius: 20rpx;
  padding: 6rpx 20rpx;
  align-self: flex-start;
}

.card-no-text {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.9);
}

.signature {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.5);
  margin-top: 10rpx;
}

.youth-card {
  position: relative;
  margin: 24rpx 32rpx;
  padding: 36rpx 40rpx;
  background: linear-gradient(135deg, #667eea, #764ba2, #f093fb);
  border-radius: 24rpx;
  overflow: hidden;
  box-shadow: 0 8rpx 40rpx rgba(102, 126, 234, 0.4);
}

.card-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(60rpx);
}

.card-orb-1 {
  width: 200rpx;
  height: 200rpx;
  background: rgba(255, 255, 255, 0.15);
  top: -60rpx;
  right: -40rpx;
}

.card-orb-2 {
  width: 160rpx;
  height: 160rpx;
  background: rgba(255, 255, 255, 0.1);
  bottom: -40rpx;
  left: 40rpx;
}

.card-deco-line {
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  height: 1rpx;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
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
  color: #FFFFFF;
}

.card-type {
  font-size: 20rpx;
  color: rgba(255, 255, 255, 0.6);
  letter-spacing: 4rpx;
}

.card-body {
  margin: 36rpx 0 32rpx;
  position: relative;
}

.card-number {
  font-size: 36rpx;
  font-weight: 700;
  color: #FFFFFF;
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
  color: rgba(255, 255, 255, 0.5);
}

.card-holder-name,
.card-expire-date {
  font-size: 26rpx;
  color: #FFFFFF;
  margin-top: 4rpx;
}

.stats-card {
  margin: 0 32rpx 24rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 24rpx;
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
  background: rgba(255, 255, 255, 0.04);
  border-radius: 24rpx;
}

.stat-num {
  font-size: 44rpx;
  font-weight: 700;
  background: linear-gradient(135deg, #667eea, #f093fb);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.stat-label {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.5);
  margin-top: 8rpx;
}

.quick-entry {
  margin: 0 32rpx 24rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 24rpx;
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
  transform: scale(0.95);
}

.entry-icon-wrap {
  width: 88rpx;
  height: 88rpx;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.entry-icon {
  font-size: 40rpx;
}

.entry-name {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.7);
  margin-top: 12rpx;
}

.menu-card {
  margin: 0 32rpx 24rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 24rpx;
  overflow: hidden;
}

.menu-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100rpx;
  padding: 0 32rpx;
  border-bottom: 1rpx solid rgba(255, 255, 255, 0.06);
}

.menu-item-last {
  border-bottom: none;
}

.menu-item-active {
  background: rgba(255, 255, 255, 0.04);
}

.menu-left {
  display: flex;
  align-items: center;
}

.menu-icon-wrap {
  width: 56rpx;
  height: 56rpx;
  border-radius: 16rpx;
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
  color: #FFFFFF;
}

.menu-arrow {
  font-size: 36rpx;
  color: rgba(255, 255, 255, 0.2);
}

.logout-btn {
  margin: 40rpx 32rpx 0;
  height: 88rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 24rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logout-btn-active {
  transform: scale(0.95);
  background: rgba(255, 77, 79, 0.1);
  border-color: rgba(255, 77, 79, 0.3);
}

.logout-text {
  font-size: 30rpx;
  color: #FF4D4F;
}
</style>
