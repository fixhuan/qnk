<template>
  <view class="page">
    <view class="search-bar">
      <view class="search-input-wrap">
        <text class="search-icon">🔍</text>
        <input class="search-input" placeholder="搜索权益" placeholder-class="search-placeholder" v-model="keyword" />
      </view>
    </view>

    <view class="card-wrap">
      <view class="youth-card">
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
const keyword = ref('')

const rights = ref<Right[]>([
  { id: 1, name: '美食8折优惠', desc: '指定餐饮商户消费享8折', icon: '🍜', bg: '#fef3ee', category: '生活权益', status: 'available', statusText: '可领取' },
  { id: 2, name: '超市满减券', desc: '满100减20，每月限领1次', icon: '🛒', bg: '#eff6ff', category: '生活权益', status: 'claimed', statusText: '已领取' },
  { id: 3, name: '健身月卡折扣', desc: '合作健身房月卡7折优惠', icon: '💪', bg: '#f0fdf4', category: '生活权益', status: 'available', statusText: '可领取' },
  { id: 4, name: '公交月卡8折', desc: '全市公交月卡享8折优惠', icon: '🚌', bg: '#eff6ff', category: '出行权益', status: 'claimed', statusText: '已领取' },
  { id: 5, name: '共享单车月卡', desc: '每月免费领取单车月卡1次', icon: '🚲', bg: '#f0fdf4', category: '出行权益', status: 'available', statusText: '可领取' },
  { id: 6, name: '地铁优惠通道', desc: '高峰期快速通行权益', icon: '🚇', bg: '#fefce8', category: '出行权益', status: 'used', statusText: '已使用' },
  { id: 7, name: '电影票买一送一', desc: '每周三电影票买一送一', icon: '🎬', bg: '#fef3ee', category: '娱乐权益', status: 'available', statusText: '可领取' },
  { id: 8, name: 'KTV欢唱券', desc: '指定KTV免费欢唱2小时', icon: '🎤', bg: '#fefce8', category: '娱乐权益', status: 'used', statusText: '已使用' },
  { id: 9, name: '景区门票半价', desc: '合作景区门票享半价优惠', icon: '🏔️', bg: '#f0fdf4', category: '娱乐权益', status: 'available', statusText: '可领取' },
  { id: 10, name: '在线课程免费学', desc: '精选课程每月3次免费学习', icon: '📚', bg: '#eff6ff', category: '教育权益', status: 'available', statusText: '可领取' }
])

const filteredRights = computed(() => {
  return rights.value.filter(item => {
    const matchCategory = item.category === currentCategory.value
    const matchKeyword = !keyword.value || item.name.includes(keyword.value) || item.desc.includes(keyword.value)
    return matchCategory && matchKeyword
  })
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
  background-color: #faf8f5;
  display: flex;
  flex-direction: column;
}

.search-bar {
  padding: 20rpx 24rpx;
}

.search-input-wrap {
  display: flex;
  align-items: center;
  background: #ffffff;
  border: 1rpx solid #e8e0d6;
  border-radius: 12rpx;
  padding: 16rpx 24rpx;
}

.search-icon {
  font-size: 28rpx;
  margin-right: 12rpx;
}

.search-input {
  flex: 1;
  font-size: 28rpx;
  color: #1a1612;
}

.search-placeholder {
  color: #a89888;
  font-size: 28rpx;
}

.card-wrap {
  padding: 24rpx;
}

.youth-card {
  background: #1a1612;
  border-radius: 12rpx;
  padding: 36rpx;
  position: relative;
  overflow: hidden;
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
  color: #c2410c;
}

.card-type {
  font-size: 22rpx;
  color: #a89888;
  letter-spacing: 4rpx;
}

.card-body {
  display: flex;
  flex-direction: column;
}

.card-no {
  font-size: 28rpx;
  color: #ffffff;
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
  color: #a89888;
  margin-bottom: 8rpx;
}

.card-info-value {
  font-size: 28rpx;
  font-weight: 600;
  color: #ffffff;
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
  background: #f5f0ea;
  margin-right: 16rpx;
}

.category-tag.active {
  background: #c2410c;
}

.category-text {
  font-size: 26rpx;
  color: #6b5e52;
}

.category-text.active-text {
  color: #ffffff;
  font-weight: 600;
}

.list-scroll {
  flex: 1;
  padding: 20rpx 24rpx;
}

.right-card {
  display: flex;
  align-items: center;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.card-hover {
  opacity: 0.6;
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
  color: #1a1612;
}

.right-desc {
  font-size: 22rpx;
  color: #a89888;
  margin-top: 6rpx;
}

.right-status {
  font-size: 22rpx;
  margin-top: 8rpx;
  color: #15803d;
}

.right-status.used {
  color: #a89888;
}

.right-status.available {
  color: #c2410c;
}

.right-btn {
  padding: 10rpx 24rpx;
  background: #c2410c;
  border-radius: 8rpx;
  flex-shrink: 0;
}

.right-btn.disabled {
  background: #f5f0ea;
}

.right-btn-text {
  font-size: 24rpx;
  color: #ffffff;
}

.right-btn-text.disabled {
  color: #a89888;
}
</style>
