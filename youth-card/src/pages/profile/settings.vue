<template>
  <view class="page">
    <view class="section">
      <text class="section-title">账号与安全</text>
      <view class="setting-card">
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('phone')">
          <view class="setting-left">
            <text class="setting-icon">📱</text>
            <text class="setting-label">手机号</text>
          </view>
          <view class="setting-right">
            <text class="setting-value">138****8888</text>
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('password')">
          <view class="setting-left">
            <text class="setting-icon">🔑</text>
            <text class="setting-label">修改密码</text>
          </view>
          <view class="setting-right">
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item setting-item-last" hover-class="setting-item-active" @tap="onSetting('verify')">
          <view class="setting-left">
            <text class="setting-icon">🪪</text>
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
            <text class="setting-icon">🔔</text>
            <text class="setting-label">消息通知</text>
          </view>
          <switch :checked="notifyEnabled" @change="onNotifyChange" color="#4F6EF7" />
        </view>
        <view class="setting-item setting-item-last">
          <view class="setting-left">
            <text class="setting-icon">🎉</text>
            <text class="setting-label">活动提醒</text>
          </view>
          <switch :checked="activityEnabled" @change="onActivityChange" color="#4F6EF7" />
        </view>
      </view>
    </view>

    <view class="section">
      <text class="section-title">通用设置</text>
      <view class="setting-card">
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('language')">
          <view class="setting-left">
            <text class="setting-icon">🌐</text>
            <text class="setting-label">语言</text>
          </view>
          <view class="setting-right">
            <text class="setting-value">简体中文</text>
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('cache')">
          <view class="setting-left">
            <text class="setting-icon">🗑️</text>
            <text class="setting-label">缓存清理</text>
          </view>
          <view class="setting-right">
            <text class="setting-value">23.5MB</text>
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item setting-item-last" hover-class="setting-item-active" @tap="onSetting('update')">
          <view class="setting-left">
            <text class="setting-icon">🔄</text>
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
            <text class="setting-icon">📄</text>
            <text class="setting-label">用户协议</text>
          </view>
          <view class="setting-right">
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item" hover-class="setting-item-active" @tap="onSetting('privacy')">
          <view class="setting-left">
            <text class="setting-icon">🛡️</text>
            <text class="setting-label">隐私政策</text>
          </view>
          <view class="setting-right">
            <text class="setting-arrow">›</text>
          </view>
        </view>
        <view class="setting-item setting-item-last" hover-class="setting-item-active" @tap="onSetting('about')">
          <view class="setting-left">
            <text class="setting-icon">ℹ️</text>
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
import { ref } from 'vue'

const notifyEnabled = ref(true)
const activityEnabled = ref(true)

const onNotifyChange = (e: any) => {
  notifyEnabled.value = e.detail.value
}

const onActivityChange = (e: any) => {
  activityEnabled.value = e.detail.value
}

const onSetting = (type: string) => {
  const messages: Record<string, string> = {
    phone: '手机号修改功能开发中',
    password: '密码修改功能开发中',
    verify: '实名认证信息页面开发中',
    language: '语言设置功能开发中',
    cache: '缓存已清理',
    update: '当前已是最新版本',
    agreement: '用户协议页面开发中',
    privacy: '隐私政策页面开发中',
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
  background-color: #F5F6FA;
  padding-bottom: 60rpx;
}

.section {
  margin-bottom: 24rpx;
}

.section-title {
  font-size: 26rpx;
  color: #999999;
  padding: 24rpx 32rpx 12rpx;
}

.setting-card {
  margin: 0 24rpx;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
  overflow: hidden;
}

.setting-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 28rpx 32rpx;
  border-bottom: 1rpx solid #F0F0F5;
}

.setting-item-last {
  border-bottom: none;
}

.setting-item-active {
  background-color: #F5F6FA;
}

.setting-left {
  display: flex;
  align-items: center;
}

.setting-icon {
  font-size: 36rpx;
  margin-right: 16rpx;
}

.setting-label {
  font-size: 28rpx;
  color: #333333;
}

.setting-right {
  display: flex;
  align-items: center;
}

.setting-value {
  font-size: 26rpx;
  color: #999999;
  margin-right: 8rpx;
}

.setting-value.verified {
  color: #4CD964;
}

.setting-arrow {
  font-size: 32rpx;
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
