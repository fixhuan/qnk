<template>
  <view class="page">
    <view :style="{ height: statusBarHeight + 'px' }"></view>
    <view class="nav-bar">
      <view class="nav-back" hover-class="nav-back-active" @tap="goBack">
        <text class="nav-back-icon">‹</text>
      </view>
      <text class="nav-title">设置</text>
      <view style="width: 60rpx;"></view>
    </view>

    <view class="section">
      <text class="section-title">账号与安全</text>
      <view class="setting-card">
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('phone')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">📱</text>
            </view>
            <text class="setting-label">手机号</text>
          </view>
          <view class="setting-right">
            <text class="setting-value">138****8888</text>
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('password')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">🔑</text>
            </view>
            <text class="setting-label">修改密码</text>
          </view>
          <view class="setting-right">
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item setting-item-last" hover-class="setting-item-active" @tap="onSetting('verify')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">🪪</text>
            </view>
            <text class="setting-label">实名认证</text>
          </view>
          <view class="setting-right">
            <text class="setting-value verified">已认证</text>
            <text class="setting-arrow">›</text>
          </view>
        </view>
      </view>
    </view>

    <view class="section">
      <text class="section-title">通知设置</text>
      <view class="setting-card">
        <view class="setting-item">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">🔔</text>
            </view>
            <text class="setting-label">消息通知</text>
          </view>
          <switch :checked="notifyEnabled" color="#c2410c" @change="toggleNotify" />
        </view>
        <view class="setting-item setting-item-last">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">🎉</text>
            </view>
            <text class="setting-label">活动提醒</text>
          </view>
          <switch :checked="activityEnabled" color="#c2410c" @change="toggleActivity" />
        </view>
      </view>
    </view>

    <view class="section">
      <text class="section-title">通用设置</text>
      <view class="setting-card">
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('language')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">🌐</text>
            </view>
            <text class="setting-label">语言</text>
          </view>
          <view class="setting-right">
            <text class="setting-value">简体中文</text>
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('cache')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">🗑️</text>
            </view>
            <text class="setting-label">缓存清理</text>
          </view>
          <view class="setting-right">
            <text class="setting-value">23.5MB</text>
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item setting-item-last" hover-class="setting-item-active" @tap="onSetting('update')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">🔄</text>
            </view>
            <text class="setting-label">检查更新</text>
          </view>
          <view class="setting-right">
            <text class="setting-value">v1.0.0</text>
            <text class="setting-arrow">›</text>
          </view>
        </view>
      </view>
    </view>

    <view class="section">
      <text class="section-title">其他</text>
      <view class="setting-card">
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('agreement')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">📄</text>
            </view>
            <text class="setting-label">用户协议</text>
          </view>
          <view class="setting-right">
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('privacy')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">🛡️</text>
            </view>
            <text class="setting-label">隐私政策</text>
          </view>
          <view class="setting-right">
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item setting-item-last" hover-class="setting-item-active" @tap="onSetting('about')">
          <view class="setting-left">
            <view class="setting-icon-wrap">
              <text class="setting-icon">ℹ️</text>
            </view>
            <text class="setting-label">关于</text>
          </view>
          <view class="setting-right">
            <text class="setting-arrow">›</text>
          </view>
        </view>
      </view>
    </view>

    <view class="logout-btn" hover-class="logout-btn-active" @tap="onLogout">
      <text class="logout-text">退出登录</text>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

const notifyEnabled = ref(true)
const activityEnabled = ref(true)
const statusBarHeight = ref(0)

onMounted(() => {
  const sysInfo = uni.getSystemInfoSync()
  statusBarHeight.value = sysInfo.statusBarHeight || 0
})

const toggleNotify = (e: any) => {
  notifyEnabled.value = e.detail.value
}

const toggleActivity = (e: any) => {
  activityEnabled.value = e.detail.value
}

const goBack = () => {
  uni.navigateBack()
}

const onSetting = (type: string) => {
  const messages: Record<string, string> = {
    phone: '手机号修改功能开发中',
    password: '密码修改功能开发中',
    verify: '实名认证信息页面开发中',
    language: '语言设置功能开发中',
    cache: '缓存已清理',
    update: '当前已是最新版本',
    agreement: '用户协议',
    privacy: '隐私政策',
    about: '关于页面开发中'
  }

  if (type === 'cache') {
    uni.showModal({
      title: '提示',
      content: '确定清理缓存吗？',
      success: (res) => {
        if (res.confirm) {
          uni.showToast({ title: messages[type], icon: 'none' })
        }
      }
    })
    return
  }

  if (type === 'about') {
    uni.navigateTo({ url: '/pages/profile/about' })
    return
  }

  if (type === 'agreement') {
    uni.showModal({
      title: '用户协议',
      content: '一、总则\n本协议是您与青年卡平台之间关于使用青年卡服务所订立的协议。\n\n二、服务内容\n青年卡平台为青年用户提供政策咨询、租房指导、求职推荐、学习规划等一站式服务。\n\n三、用户权利\n1. 用户有权使用平台提供的各项服务\n2. 用户有权对平台服务提出意见和建议\n3. 用户有权随时注销账户\n\n四、用户义务\n1. 用户应提供真实、准确的个人信息\n2. 用户应遵守平台使用规则\n3. 用户不得利用平台从事违法活动',
      showCancel: false,
      confirmText: '我知道了'
    })
    return
  }

  if (type === 'privacy') {
    uni.showModal({
      title: '隐私政策',
      content: '一、信息收集\n我们可能收集以下信息：\n1. 注册信息：姓名、手机号、身份证号\n2. 使用数据：浏览记录、搜索记录\n3. 设备信息：设备型号、操作系统版本\n\n二、信息使用\n收集的信息将用于：\n1. 提供和改进服务\n2. 个性化推荐\n3. 安全防护\n\n三、信息保护\n1. 采用加密存储和传输\n2. 严格权限管理\n3. 定期安全审计\n\n四、用户权利\n1. 查询个人信息\n2. 更正个人信息\n3. 删除个人信息\n4. 撤回授权同意',
      showCancel: false,
      confirmText: '我知道了'
    })
    return
  }

  uni.showToast({ title: messages[type] || '功能开发中', icon: 'none' })
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

.section {
  margin-bottom: 24rpx;
}

.section-title {
  font-size: 26rpx;
  color: #a89888;
  padding: 24rpx 32rpx 12rpx;
}

.setting-card {
  margin: 0 32rpx;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  overflow: hidden;
}

.setting-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 28rpx 32rpx;
  border-bottom: 1rpx solid #f0ebe3;
}

.setting-item-last {
  border-bottom: none;
}

.setting-item-active {
  background: #f5f0ea;
}

.setting-left {
  display: flex;
  align-items: center;
}

.setting-icon-wrap {
  width: 52rpx;
  height: 52rpx;
  border-radius: 50%;
  background: #f5f0ea;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 16rpx;
}

.setting-icon {
  font-size: 28rpx;
}

.setting-label {
  font-size: 28rpx;
  color: #1a1612;
}

.setting-right {
  display: flex;
  align-items: center;
}

.setting-value {
  font-size: 26rpx;
  color: #a89888;
  margin-right: 8rpx;
}

.setting-value.verified {
  color: #15803d;
}

.setting-arrow {
  font-size: 32rpx;
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
