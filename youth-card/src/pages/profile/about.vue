<template>
  <view class="page">
    <view :style="{ height: statusBarHeight + 'px' }"></view>
    <view class="nav-bar">
      <view class="nav-back" hover-class="nav-back-active" @tap="goBack">
        <text class="nav-back-icon">‹</text>
      </view>
      <text class="nav-title">关于我们</text>
      <view style="width: 60rpx;"></view>
    </view>

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
        <view class="feature-icon-wrap" :style="{ background: feature.bgColor }">
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
import { ref, onMounted } from 'vue'

const features = ref([
  { icon: '🎯', title: '精准服务', desc: '基于青年需求，提供政策、租房、求职等一站式服务', bgColor: '#fef3ee' },
  { icon: '🔒', title: '安全可靠', desc: '政府背书，数据加密，保障您的信息安全', bgColor: '#f0fdf4' },
  { icon: '⚡', title: '便捷高效', desc: '在线申请，一键办理，让服务触手可及', bgColor: '#eff6ff' },
  { icon: '🌟', title: '专属优惠', desc: '青年卡用户专享折扣和补贴，助力青年发展', bgColor: '#fefce8' }
])

const statusBarHeight = ref(0)

onMounted(() => {
  const sysInfo = uni.getSystemInfoSync()
  statusBarHeight.value = sysInfo.statusBarHeight || 0
})

const goBack = () => {
  uni.navigateBack()
}

const onLink = (type: string) => {
  const title = type === 'user' ? '用户协议' : '隐私政策'
  const content = type === 'user'
    ? '一、总则\n本协议是您与青年卡平台之间关于使用青年卡服务所订立的协议。\n\n二、服务内容\n青年卡平台为青年用户提供政策咨询、租房指导、求职推荐、学习规划等一站式服务。\n\n三、用户权利\n1. 用户有权使用平台提供的各项服务\n2. 用户有权对平台服务提出意见和建议\n3. 用户有权随时注销账户\n\n四、用户义务\n1. 用户应提供真实、准确的个人信息\n2. 用户应遵守平台使用规则\n3. 用户不得利用平台从事违法活动\n\n五、隐私保护\n平台将严格保护用户个人隐私，详见《隐私政策》。'
    : '一、信息收集\n我们可能收集以下信息：\n1. 注册信息：姓名、手机号、身份证号\n2. 使用数据：浏览记录、搜索记录\n3. 设备信息：设备型号、操作系统版本\n\n二、信息使用\n收集的信息将用于：\n1. 提供和改进服务\n2. 个性化推荐\n3. 安全防护\n\n三、信息保护\n1. 采用加密存储和传输\n2. 严格权限管理\n3. 定期安全审计\n\n四、用户权利\n1. 查询个人信息\n2. 更正个人信息\n3. 删除个人信息\n4. 撤回授权同意'
  uni.showModal({ title, content, showCancel: false, confirmText: '我知道了' })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #faf8f5;
  padding-bottom: 60rpx;
}

.nav-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16rpx 32rpx;
  background-color: #faf8f5;
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
  color: #1a1612;
}

.nav-title {
  font-size: 34rpx;
  font-weight: 700;
  color: #1a1612;
}

.logo-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 48rpx 0;
}

.logo-circle {
  width: 160rpx;
  height: 160rpx;
  border-radius: 50%;
  background: #fef3ee;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo-icon {
  font-size: 72rpx;
}

.app-name {
  font-size: 40rpx;
  font-weight: 700;
  color: #1a1612;
  margin-top: 24rpx;
}

.app-version {
  font-size: 26rpx;
  color: #a89888;
  margin-top: 8rpx;
}

.features-card {
  margin: 0 32rpx 24rpx;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
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
  border-radius: 50%;
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
  color: #1a1612;
}

.feature-desc {
  font-size: 24rpx;
  color: #6b5e52;
  margin-top: 6rpx;
  line-height: 1.5;
}

.contact-card {
  margin: 0 32rpx 24rpx;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  padding: 32rpx;
}

.card-title {
  font-size: 30rpx;
  font-weight: 600;
  color: #1a1612;
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
  border-radius: 50%;
  background: #f5f0ea;
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
  color: #6b5e52;
}

.links-card {
  margin: 0 32rpx 24rpx;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  overflow: hidden;
}

.link-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 28rpx 32rpx;
  border-bottom: 1rpx solid #f0ebe3;
}

.link-item:last-child {
  border-bottom: none;
}

.link-item-active {
  background: #f5f0ea;
}

.link-text {
  font-size: 28rpx;
  color: #c2410c;
}

.link-arrow {
  font-size: 32rpx;
  color: #e8e0d6;
}

.copyright {
  text-align: center;
  padding: 40rpx 0;
}

.copyright-text {
  font-size: 22rpx;
  color: #a89888;
}
</style>
