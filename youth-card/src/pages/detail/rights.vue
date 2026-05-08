<template>
  <view class="page">
    <view class="card-wrap">
      <view class="youth-card">
        <view class="card-accent"></view>
        <view class="card-content">
          <view class="card-top">
            <text class="card-label">青年卡</text>
            <text class="card-type">YOUTH CARD</text>
          </view>
          <view class="card-body">
            <text class="card-no">No. YC2026001234</text>
            <view class="card-info-row">
              <view class="card-info-item">
                <text class="card-info-label">持卡人</text>
                <text class="card-info-value">张**</text>
              </view>
              <view class="card-info-item">
                <text class="card-info-label">有效期</text>
                <text class="card-info-value">2026-12-31</text>
              </view>
            </view>
          </view>
        </view>
      </view>
    </view>

    <scroll-view class="category-scroll" scroll-x :show-scrollbar="false">
      <view
        class="category-tag"
        :class="{ active: currentCategory === item }"
        v-for="item in categories"
        :key="item"
        @click="currentCategory = item"
      >
        <text class="category-text" :class="{ 'active-text': currentCategory === item }">{{ item }}</text>
      </view>
    </scroll-view>

    <scroll-view class="list-scroll" scroll-y>
      <view class="right-card" v-for="item in filteredRights" :key="item.id" hover-class="card-hover">
        <view class="right-icon" :style="{ background: item.bg }">
          <text class="right-icon-text">{{ item.icon }}</text>
        </view>
        <view class="right-info">
          <text class="right-name">{{ item.name }}</text>
          <text class="right-desc">{{ item.desc }}</text>
          <text class="right-status" :class="{ used: item.status === 'used', available: item.status === 'available' }">{{ item.statusText }}</text>
        </view>
        <view class="right-btn" :class="{ disabled: item.status !== 'available' }" @click="onUseRight(item)">
          <text class="right-btn-text" :class="{ disabled: item.status !== 'available' }">{{ item.status === 'available' ? '领取' : item.status === 'used' ? '已使用' : '已领取' }}</text>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Right {
  id: number
  name: string
  desc: string
  icon: string
  bg: string
  category: string
  status: string
  statusText: string
}

const currentCategory = ref('生活权益')
const categories = ['生活权益', '出行权益', '娱乐权益', '教育权益']

const rights = ref<Right[]>([
  { id: 1, name: '美食8折优惠', desc: '指定餐饮商户消费享8折', icon: '🍜', bg: 'rgba(65,108,129,0.08)', category: '生活权益', status: 'available', statusText: '可领取' },
  { id: 2, name: '超市满减券', desc: '满100减20，每月限领1次', icon: '🛒', bg: 'rgba(90,200,250,0.08)', category: '生活权益', status: 'claimed', statusText: '已领取' },
  { id: 3, name: '健身月卡折扣', desc: '合作健身房月卡7折优惠', icon: '💪', bg: 'rgba(52,199,89,0.08)', category: '生活权益', status: 'available', statusText: '可领取' },
  { id: 4, name: '公交月卡8折', desc: '全市公交月卡享8折优惠', icon: '🚌', bg: 'rgba(90,200,250,0.08)', category: '出行权益', status: 'claimed', statusText: '已领取' },
  { id: 5, name: '共享单车月卡', desc: '每月免费领取单车月卡1次', icon: '🚲', bg: 'rgba(52,199,89,0.08)', category: '出行权益', status: 'available', statusText: '可领取' },
  { id: 6, name: '地铁优惠通道', desc: '高峰期快速通行权益', icon: '🚇', bg: 'rgba(255,149,0,0.08)', category: '出行权益', status: 'used', statusText: '已使用' },
  { id: 7, name: '电影票买一送一', desc: '每周三电影票买一送一', icon: '🎬', bg: 'rgba(65,108,129,0.08)', category: '娱乐权益', status: 'available', statusText: '可领取' },
  { id: 8, name: 'KTV欢唱券', desc: '指定KTV免费欢唱2小时', icon: '🎤', bg: 'rgba(255,149,0,0.08)', category: '娱乐权益', status: 'used', statusText: '已使用' },
  { id: 9, name: '景区门票半价', desc: '合作景区门票享半价优惠', icon: '🏔️', bg: 'rgba(52,199,89,0.08)', category: '娱乐权益', status: 'available', statusText: '可领取' },
  { id: 10, name: '在线课程免费学', desc: '精选课程每月3次免费学习', icon: '📚', bg: 'rgba(90,200,250,0.08)', category: '教育权益', status: 'available', statusText: '可领取' }
])

const filteredRights = computed(() => {
  return rights.value.filter(item => item.category === currentCategory.value)
})

const onUseRight = (item: Right) => {
  if (item.status !== 'available') return
  item.status = 'claimed'
  item.statusText = '已领取'
  uni.showToast({ title: '领取成功', icon: 'success' })
}
</script>

<style>
.page {
  min-height: 100vh;
  background-color: #F1F3F4;
  display: flex;
  flex-direction: column;
}

.card-wrap {
  padding: 24rpx;
}

.youth-card {
  display: flex;
  background: #FFFFFF;
  border-radius: 16rpx;
  overflow: hidden;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
}

.card-accent {
  width: 8rpx;
  background: #416C81;
  flex-shrink: 0;
}

.card-content {
  flex: 1;
  padding: 36rpx;
}

.card-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 40rpx;
}

.card-label {
  font-size: 36rpx;
  font-weight: 700;
  color: #416C81;
}

.card-type {
  font-size: 22rpx;
  color: #AEAEB2;
  letter-spacing: 4rpx;
}

.card-body {
  display: flex;
  flex-direction: column;
}

.card-no {
  font-size: 28rpx;
  color: #1C1C1E;
  letter-spacing: 2rpx;
  margin-bottom: 32rpx;
}

.card-info-row {
  display: flex;
  gap: 60rpx;
}

.card-info-item {
  display: flex;
  flex-direction: column;
}

.card-info-label {
  font-size: 22rpx;
  color: #AEAEB2;
  margin-bottom: 8rpx;
}

.card-info-value {
  font-size: 28rpx;
  font-weight: 600;
  color: #1C1C1E;
}

.category-scroll {
  white-space: nowrap;
  padding: 16rpx 24rpx;
}

.category-tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12rpx 32rpx;
  border-radius: 999rpx;
  background: #F8F9FA;
  margin-right: 16rpx;
}

.category-tag.active {
  background: #416C81;
}

.category-text {
  font-size: 26rpx;
  color: #8E8E93;
}

.category-text.active-text {
  color: #FFFFFF;
  font-weight: 600;
}

.list-scroll {
  flex: 1;
  padding: 20rpx 24rpx;
}

.right-card {
  display: flex;
  align-items: center;
  background: #FFFFFF;
  border-radius: 16rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
}

.card-hover {
  background: #F8F9FA;
}

.right-icon {
  width: 80rpx;
  height: 80rpx;
  border-radius: 12rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.right-icon-text {
  font-size: 36rpx;
}

.right-info {
  flex: 1;
  margin-left: 20rpx;
}

.right-name {
  font-size: 28rpx;
  font-weight: 600;
  color: #1C1C1E;
}

.right-desc {
  font-size: 22rpx;
  color: #AEAEB2;
  margin-top: 6rpx;
}

.right-status {
  font-size: 22rpx;
  margin-top: 8rpx;
  color: #248A3D;
}

.right-status.used {
  color: #AEAEB2;
}

.right-status.available {
  color: #416C81;
}

.right-btn {
  padding: 10rpx 24rpx;
  background: #416C81;
  border-radius: 12rpx;
  flex-shrink: 0;
}

.right-btn.disabled {
  background: #F8F9FA;
}

.right-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
}

.right-btn-text.disabled {
  color: #8E8E93;
}
</style>
