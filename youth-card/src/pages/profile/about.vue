<template>
  <view class="page">
    <view class="logo-area">
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
        <text class="feature-icon">{{ feature.icon }}</text>
        <view class="feature-text">
          <text class="feature-title">{{ feature.title }}</text>
          <text class="feature-desc">{{ feature.desc }}</text>
        </view>
      </view>
    </view>

    <view class="contact-card">
      <text class="card-title">联系我们</text>
      <view class="contact-item">
        <text class="contact-icon">📞</text>
        <text class="contact-text">客服电话：400-888-9999</text>
      </view>
      <view class="contact-item">
        <text class="contact-icon">📧</text>
        <text class="contact-text">邮箱：service@youthcard.cn</text>
      </view>
      <view class="contact-item">
        <text class="contact-icon">📍</text>
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
  { icon: '🎯', title: '精准服务', desc: '基于青年需求，提供政策、租房、求职等一站式服务' },
  { icon: '🔒', title: '安全可靠', desc: '政府背书，数据加密，保障您的信息安全' },
  { icon: '⚡', title: '便捷高效', desc: '在线申请，一键办理，让服务触手可及' },
  { icon: '🌟', title: '专属优惠', desc: '青年卡用户专享折扣和补贴，助力青年发展' }
])

const onLink = (type: string) => {
  const title = type === 'user' ? '用户协议' : '隐私政策'
  uni.showToast({ title: `${title}页面开发中`, icon: 'none' })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #F5F6FA;
  padding-bottom: 60rpx;
}

.logo-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 60rpx 0 40rpx;
}

.logo-circle {
  width: 160rpx;
  height: 160rpx;
  border-radius: 50%;
  background: linear-gradient(135deg, #4F6EF7, #7B93FA);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8rpx 32rpx rgba(79, 110, 247, 0.3);
}

.logo-icon {
  font-size: 72rpx;
}

.app-name {
  font-size: 40rpx;
  font-weight: 700;
  color: #333333;
  margin-top: 24rpx;
}

.app-version {
  font-size: 26rpx;
  color: #999999;
  margin-top: 8rpx;
}

.features-card {
  margin: 0 24rpx 24rpx;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
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

.feature-icon {
  font-size: 44rpx;
  margin-right: 20rpx;
  flex-shrink: 0;
}

.feature-text {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.feature-title {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
}

.feature-desc {
  font-size: 24rpx;
  color: #999999;
  margin-top: 6rpx;
  line-height: 1.5;
}

.contact-card {
  margin: 0 24rpx 24rpx;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
  padding: 32rpx;
}

.card-title {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
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

.contact-icon {
  font-size: 36rpx;
  margin-right: 16rpx;
  flex-shrink: 0;
}

.contact-text {
  font-size: 28rpx;
  color: #666666;
}

.links-card {
  margin: 0 24rpx 24rpx;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
  overflow: hidden;
}

.link-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 28rpx 32rpx;
  border-bottom: 1rpx solid #F0F0F5;
}

.link-item:last-child {
  border-bottom: none;
}

.link-item-active {
  background-color: #F5F6FA;
}

.link-text {
  font-size: 28rpx;
  color: #4F6EF7;
}

.link-arrow {
  font-size: 32rpx;
  color: #CCCCCC;
}

.copyright {
  text-align: center;
  padding: 40rpx 0;
}

.copyright-text {
  font-size: 22rpx;
  color: #CCCCCC;
}
</style>
