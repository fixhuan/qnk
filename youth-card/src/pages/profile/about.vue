<template>
  <view class="page">
    <view class="nav-bar">
      <view class="nav-back" hover-class="nav-back-active" @tap="goBack">
        <text class="nav-back-icon">‹</text>
      </view>
      <text class="nav-title">关于我们</text>
      <view style="width: 60rpx;"></view>
    </view>

    <view class="logo-area">
      <view class="logo-orb logo-orb-1"></view>
      <view class="logo-orb logo-orb-2"></view>
      <view class="logo-circle">
        <text class="logo-icon">💳</text>
      </view>
      <text class="app-name">青年卡</text>
      <text class="app-version">版本 1.0.0</text>
    </view>

    <view class="features-card">
      <view
        v-for="(feature, index) in features"
        :key="index"
        class="feature-item"
      >
        <view class="feature-icon-wrap" :style="{ background: feature.gradient }">
          <text class="feature-icon">{{ feature.icon }}</text>
        </view>
        <view class="feature-text">
          <text class="feature-title">{{ feature.title }}</text>
          <text class="feature-desc">{{ feature.desc }}</text>
        </view>
      </view>
    </view>

    <view class="contact-card">
      <text class="card-title">联系我们</text>
      <view class="contact-item">
        <view class="contact-icon-wrap">
          <text class="contact-icon">📞</text>
        </view>
        <text class="contact-text">客服电话：400-888-9999</text>
      </view>
      <view class="contact-item">
        <view class="contact-icon-wrap">
          <text class="contact-icon">📧</text>
        </view>
        <text class="contact-text">邮箱：service@youthcard.cn</text>
      </view>
      <view class="contact-item">
        <view class="contact-icon-wrap">
          <text class="contact-icon">📍</text>
        </view>
        <text class="contact-text">地址：高新区青年创新大厦12楼</text>
      </view>
    </view>

    <view class="links-card">
      <view class="link-item" hover-class="link-item-active" @tap="onLink('user')">
        <text class="link-text">用户协议</text>
        <text class="link-arrow">›</text>
      </view>
      <view class="link-item" hover-class="link-item-active" @tap="onLink('privacy')">
        <text class="link-text">隐私政策</text>
        <text class="link-arrow">›</text>
      </view>
    </view>

    <view class="copyright">
      <text class="copyright-text">© 2025 青年卡 All Rights Reserved</text>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const features = ref([
  { icon: '🎯', title: '精准服务', desc: '基于青年需求，提供政策、租房、求职等一站式服务', gradient: 'linear-gradient(135deg, #667eea, #764ba2)' },
  { icon: '🔒', title: '安全可靠', desc: '政府背书，数据加密，保障您的信息安全', gradient: 'linear-gradient(135deg, #4facfe, #00f2fe)' },
  { icon: '⚡', title: '便捷高效', desc: '在线申请，一键办理，让服务触手可及', gradient: 'linear-gradient(135deg, #f093fb, #f5576c)' },
  { icon: '🌟', title: '专属优惠', desc: '青年卡用户专享折扣和补贴，助力青年发展', gradient: 'linear-gradient(135deg, #43e97b, #38f9d7)' }
])

const goBack = () => {
  uni.navigateBack()
}

const onLink = (type: string) => {
  const title = type === 'user' ? '用户协议' : '隐私政策'
  uni.showToast({ title: `${title}页面开发中`, icon: 'none' })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #0f0f2d;
  padding-bottom: 60rpx;
}

.nav-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 60rpx 24rpx 16rpx;
  background-color: #0f0f2d;
}

.nav-back {
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-back-active {
  opacity: 0.6;
}

.nav-back-icon {
  font-size: 44rpx;
  color: #FFFFFF;
}

.nav-title {
  font-size: 34rpx;
  font-weight: 700;
  color: #FFFFFF;
}

.logo-area {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40rpx 0;
  overflow: hidden;
}

.logo-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80rpx);
}

.logo-orb-1 {
  width: 300rpx;
  height: 300rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  opacity: 0.25;
  top: -80rpx;
  left: 50%;
  transform: translateX(-50%);
}

.logo-orb-2 {
  width: 200rpx;
  height: 200rpx;
  background: linear-gradient(135deg, #f093fb, #f5576c);
  opacity: 0.15;
  top: 40rpx;
  right: 20%;
}

.logo-circle {
  width: 160rpx;
  height: 160rpx;
  border-radius: 50%;
  background: linear-gradient(135deg, #667eea, #764ba2);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 0 40rpx rgba(124, 92, 252, 0.5);
  position: relative;
}

.logo-icon {
  font-size: 72rpx;
}

.app-name {
  font-size: 40rpx;
  font-weight: 700;
  color: #FFFFFF;
  margin-top: 24rpx;
  position: relative;
}

.app-version {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.4);
  margin-top: 8rpx;
  position: relative;
}

.features-card {
  margin: 0 32rpx 24rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 24rpx;
  padding: 32rpx;
}

.feature-item {
  display: flex;
  align-items: flex-start;
  margin-bottom: 28rpx;
}

.feature-item:last-child {
  margin-bottom: 0;
}

.feature-icon-wrap {
  width: 64rpx;
  height: 64rpx;
  border-radius: 18rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 20rpx;
  flex-shrink: 0;
}

.feature-icon {
  font-size: 34rpx;
}

.feature-text {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.feature-title {
  font-size: 30rpx;
  font-weight: 600;
  color: #FFFFFF;
}

.feature-desc {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.4);
  margin-top: 6rpx;
  line-height: 1.5;
}

.contact-card {
  margin: 0 32rpx 24rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 24rpx;
  padding: 32rpx;
}

.card-title {
  font-size: 30rpx;
  font-weight: 600;
  color: #FFFFFF;
  margin-bottom: 24rpx;
}

.contact-item {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
}

.contact-item:last-child {
  margin-bottom: 0;
}

.contact-icon-wrap {
  width: 52rpx;
  height: 52rpx;
  border-radius: 14rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 16rpx;
  flex-shrink: 0;
}

.contact-icon {
  font-size: 28rpx;
}

.contact-text {
  font-size: 28rpx;
  color: rgba(255, 255, 255, 0.6);
}

.links-card {
  margin: 0 32rpx 24rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 24rpx;
  overflow: hidden;
}

.link-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 28rpx 32rpx;
  border-bottom: 1rpx solid rgba(255, 255, 255, 0.06);
}

.link-item:last-child {
  border-bottom: none;
}

.link-item-active {
  background: rgba(255, 255, 255, 0.04);
}

.link-text {
  font-size: 28rpx;
  color: #7c5cfc;
}

.link-arrow {
  font-size: 32rpx;
  color: rgba(255, 255, 255, 0.2);
}

.copyright {
  text-align: center;
  padding: 40rpx 0;
}

.copyright-text {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.2);
}
</style>
