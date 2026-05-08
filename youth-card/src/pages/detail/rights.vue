<template>
  <view class="page">
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
      <view class="right-card" v-for="item in filteredRights" :key="item.id">
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
  { id: 1, name: '美食8折优惠', desc: '指定餐饮商户消费享8折', icon: '🍜', bg: 'linear-gradient(135deg, #FF6B6B 0%, #FF8E8E 100%)', category: '生活权益', status: 'available', statusText: '可领取' },
  { id: 2, name: '超市满减券', desc: '满100减20，每月限领1次', icon: '🛒', bg: 'linear-gradient(135deg, #4F6EF7 0%, #7B9AFF 100%)', category: '生活权益', status: 'claimed', statusText: '已领取' },
  { id: 3, name: '健身月卡折扣', desc: '合作健身房月卡7折优惠', icon: '💪', bg: 'linear-gradient(135deg, #2ECC71 0%, #58D68D 100%)', category: '生活权益', status: 'available', statusText: '可领取' },
  { id: 4, name: '公交月卡8折', desc: '全市公交月卡享8折优惠', icon: '🚌', bg: 'linear-gradient(135deg, #4FC7F7 0%, #88D8FF 100%)', category: '出行权益', status: 'claimed', statusText: '已领取' },
  { id: 5, name: '共享单车月卡', desc: '每月免费领取单车月卡1次', icon: '🚲', bg: 'linear-gradient(135deg, #2ECC71 0%, #58D68D 100%)', category: '出行权益', status: 'available', statusText: '可领取' },
  { id: 6, name: '地铁优惠通道', desc: '高峰期快速通行权益', icon: '🚇', bg: 'linear-gradient(135deg, #9B59B6 0%, #C39BD3 100%)', category: '出行权益', status: 'used', statusText: '已使用' },
  { id: 7, name: '电影票买一送一', desc: '每周三电影票买一送一', icon: '🎬', bg: 'linear-gradient(135deg, #F7854F 0%, #FFB088 100%)', category: '娱乐权益', status: 'available', statusText: '可领取' },
  { id: 8, name: 'KTV欢唱券', desc: '指定KTV免费欢唱2小时', icon: '🎤', bg: 'linear-gradient(135deg, #FF6B6B 0%, #FF8E8E 100%)', category: '娱乐权益', status: 'used', statusText: '已使用' },
  { id: 9, name: '景区门票半价', desc: '合作景区门票享半价优惠', icon: '🏔️', bg: 'linear-gradient(135deg, #4F6EF7 0%, #7B9AFF 100%)', category: '娱乐权益', status: 'available', statusText: '可领取' },
  { id: 10, name: '在线课程免费学', desc: '精选课程每月3次免费学习', icon: '📚', bg: 'linear-gradient(135deg, #9B59B6 0%, #C39BD3 100%)', category: '教育权益', status: 'available', statusText: '可领取' }
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
  background-color: #F5F6FA;
  display: flex;
  flex-direction: column;
}

.card-wrap {
  padding: 24rpx;
}

.youth-card {
  background: linear-gradient(135deg, #4F6EF7 0%, #3A56D4 50%, #7B9AFF 100%);
  border-radius: 24rpx;
  padding: 36rpx;
  box-shadow: 0 12rpx 32rpx rgba(79, 110, 247, 0.3);
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
  color: #FFFFFF;
}

.card-type {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.6);
  letter-spacing: 4rpx;
}

.card-body {
  display: flex;
  flex-direction: column;
}

.card-no {
  font-size: 28rpx;
  color: rgba(255, 255, 255, 0.9);
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
  color: rgba(255, 255, 255, 0.6);
  margin-bottom: 8rpx;
}

.card-info-value {
  font-size: 28rpx;
  font-weight: 600;
  color: #FFFFFF;
}

.category-scroll {
  white-space: nowrap;
  background-color: #FFFFFF;
  padding: 16rpx 24rpx;
  border-bottom: 1rpx solid #F0F0F0;
}

.category-tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12rpx 32rpx;
  border-radius: 32rpx;
  background-color: #F0F2F5;
  margin-right: 16rpx;
}

.category-tag.active {
  background-color: #4F6EF7;
}

.category-text {
  font-size: 26rpx;
  color: #666666;
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
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.right-icon {
  width: 80rpx;
  height: 80rpx;
  border-radius: 20rpx;
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
  font-weight: 700;
  color: #333333;
}

.right-desc {
  font-size: 22rpx;
  color: #999999;
  margin-top: 6rpx;
}

.right-status {
  font-size: 22rpx;
  margin-top: 8rpx;
  color: #4F6EF7;
}

.right-status.used {
  color: #CCCCCC;
}

.right-status.available {
  color: #2ECC71;
}

.right-btn {
  padding: 10rpx 24rpx;
  background-color: #4F6EF7;
  border-radius: 28rpx;
  flex-shrink: 0;
}

.right-btn.disabled {
  background-color: #E0E0E0;
}

.right-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
}

.right-btn-text.disabled {
  color: #999999;
}
</style>
