<template>
  <view class="page">
    <view class="nav-bar">
      <view class="nav-back" hover-class="nav-back-active" @tap="goBack">
        <text class="nav-back-icon">‹</text>
      </view>
      <text class="nav-title">我的订单</text>
      <view style="width: 60rpx;"></view>
    </view>

    <view class="tabs">
      <view
        v-for="(tab, index) in tabs"
        :key="index"
        class="tab-item"
        :class="{ 'tab-active': currentTab === index }"
        @tap="switchTab(index)"
      >
        <text class="tab-text" :class="{ 'tab-text-active': currentTab === index }">{{ tab }}</text>
        <view v-if="currentTab === index" class="tab-indicator"></view>
      </view>
    </view>

    <scroll-view class="order-list" scroll-y>
      <view
        v-for="(order, index) in filteredOrders"
        :key="index"
        class="order-card"
      >
        <view class="order-header">
          <text class="order-shop">{{ order.shop }}</text>
          <text class="order-status" :class="statusClass(order.status)">{{ order.statusText }}</text>
        </view>
        <view class="order-body">
          <text class="order-no">订单编号：{{ order.orderNo }}</text>
          <text class="order-time">下单时间：{{ order.time }}</text>
        </view>
        <view class="order-footer">
          <text class="order-price">¥{{ order.price }}</text>
          <view class="order-actions">
            <view
              v-if="order.status === 0"
              class="action-btn action-btn-primary"
              hover-class="action-btn-active"
              @tap="onPay(order)"
            >
              <text class="action-btn-text-primary">付款</text>
            </view>
            <view
              v-if="order.status === 3"
              class="action-btn action-btn-primary"
              hover-class="action-btn-active"
              @tap="onReview(order)"
            >
              <text class="action-btn-text-primary">评价</text>
            </view>
            <view
              v-if="order.status === 2"
              class="action-btn action-btn-primary"
              hover-class="action-btn-active"
              @tap="onUse(order)"
            >
              <text class="action-btn-text-primary">使用</text>
            </view>
            <view
              v-if="order.status === 3 || order.status === 4"
              class="action-btn action-btn-default"
              hover-class="action-btn-active"
              @tap="onDelete(order)"
            >
              <text class="action-btn-text-default">删除</text>
            </view>
          </view>
        </view>
      </view>

      <view v-if="filteredOrders.length === 0" class="empty">
        <text class="empty-icon">📦</text>
        <text class="empty-text">暂无订单</text>
      </view>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Order {
  shop: string
  orderNo: string
  price: string
  status: number
  statusText: string
  time: string
}

const tabs = ['全部', '待付款', '待使用', '已完成', '已取消']
const currentTab = ref(0)

const orders = ref<Order[]>([
  { shop: '青年夜校·Python编程课', orderNo: 'YQ20250101001', price: '299.00', status: 0, statusText: '待付款', time: '2025-05-08 14:30' },
  { shop: '高新区青年公寓·月租', orderNo: 'YQ20250430002', price: '1200.00', status: 2, statusText: '待使用', time: '2025-04-30 09:15' },
  { shop: '星巴克·青年卡专享', orderNo: 'YQ20250428003', price: '25.00', status: 3, statusText: '已完成', time: '2025-04-28 16:20' },
  { shop: '青年健身房·季卡', orderNo: 'YQ20250425004', price: '599.00', status: 2, statusText: '待使用', time: '2025-04-25 11:00' },
  { shop: 'UI设计进阶课', orderNo: 'YQ20250420005', price: '499.00', status: 3, statusText: '已完成', time: '2025-04-20 08:45' },
  { shop: '新华书店·购书优惠', orderNo: 'YQ20250415006', price: '68.00', status: 4, statusText: '已取消', time: '2025-04-15 13:30' },
  { shop: '青年创业沙龙门票', orderNo: 'YQ20250410007', price: '0.00', status: 3, statusText: '已完成', time: '2025-04-10 10:00' },
  { shop: '新媒体运营实战课', orderNo: 'YQ20250405008', price: '399.00', status: 0, statusText: '待付款', time: '2025-04-05 17:20' },
  { shop: '海底捞·青年卡折扣', orderNo: 'YQ20250403009', price: '156.00', status: 3, statusText: '已完成', time: '2025-04-03 19:45' },
  { shop: '英语口语提升班', orderNo: 'YQ20250401010', price: '899.00', status: 4, statusText: '已取消', time: '2025-04-01 14:10' }
])

const filteredOrders = computed(() => {
  if (currentTab.value === 0) return orders.value
  return orders.value.filter(o => o.status === currentTab.value - 1)
})

const switchTab = (index: number) => {
  currentTab.value = index
}

const statusClass = (status: number) => {
  const map: Record<number, string> = {
    0: 'status-pending',
    1: 'status-pending',
    2: 'status-using',
    3: 'status-done',
    4: 'status-cancel'
  }
  return map[status] || ''
}

const goBack = () => {
  uni.navigateBack()
}

const onPay = (order: Order) => {
  uni.showToast({ title: `支付 ¥${order.price}`, icon: 'none' })
}

const onReview = (order: Order) => {
  uni.showToast({ title: '评价功能开发中', icon: 'none' })
}

const onUse = (order: Order) => {
  uni.showToast({ title: '已确认使用', icon: 'none' })
}

const onDelete = (order: Order) => {
  uni.showModal({
    title: '提示',
    content: '确定删除该订单吗？',
    success: (res) => {
      if (res.confirm) {
        const idx = orders.value.indexOf(order)
        if (idx > -1) orders.value.splice(idx, 1)
        uni.showToast({ title: '已删除', icon: 'none' })
      }
    }
  })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #F1F3F4;
}

.nav-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 60rpx 32rpx 16rpx;
  background-color: #F1F3F4;
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
  color: #1C1C1E;
}

.nav-title {
  font-size: 34rpx;
  font-weight: 700;
  color: #1C1C1E;
}

.tabs {
  display: flex;
  padding: 16rpx 32rpx;
  background-color: #F1F3F4;
}

.tab-item {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20rpx 0;
  position: relative;
  flex-direction: column;
}

.tab-indicator {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 40rpx;
  height: 3rpx;
  background: #416C81;
  border-radius: 2rpx;
}

.tab-text {
  font-size: 26rpx;
  color: #8E8E93;
}

.tab-text-active {
  color: #416C81;
  font-weight: 600;
}

.order-list {
  height: calc(100vh - 180rpx);
  padding: 0 32rpx 32rpx;
}

.order-card {
  background: #FFFFFF;
  border-radius: 16rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
  padding: 28rpx;
  margin-bottom: 20rpx;
}

.order-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16rpx;
}

.order-shop {
  font-size: 30rpx;
  font-weight: 600;
  color: #1C1C1E;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.order-status {
  font-size: 24rpx;
  padding: 6rpx 20rpx;
  border-radius: 999rpx;
  flex-shrink: 0;
  margin-left: 16rpx;
}

.status-pending {
  color: #C93400;
  background: rgba(255,149,0,0.08);
}

.status-using {
  color: #007AFF;
  background: rgba(90,200,250,0.08);
}

.status-done {
  color: #248A3D;
  background: rgba(52,199,89,0.08);
}

.status-cancel {
  color: #8E8E93;
  background: #F8F9FA;
}

.order-body {
  display: flex;
  flex-direction: column;
  gap: 8rpx;
  padding-bottom: 20rpx;
  border-bottom: 0.5rpx solid #E5E5EA;
}

.order-no {
  font-size: 24rpx;
  color: #8E8E93;
}

.order-time {
  font-size: 24rpx;
  color: #8E8E93;
}

.order-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20rpx;
}

.order-price {
  font-size: 34rpx;
  font-weight: 700;
  color: #1C1C1E;
}

.order-actions {
  display: flex;
  gap: 16rpx;
}

.action-btn {
  padding: 10rpx 28rpx;
  border-radius: 12rpx;
}

.action-btn-primary {
  background: #416C81;
}

.action-btn-default {
  background: #F8F9FA;
}

.action-btn-active {
  opacity: 0.6;
}

.action-btn-text-primary {
  font-size: 24rpx;
  color: #FFFFFF;
  font-weight: 600;
}

.action-btn-text-default {
  font-size: 24rpx;
  color: #8E8E93;
}

.empty {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 200rpx;
}

.empty-icon {
  font-size: 100rpx;
  margin-bottom: 24rpx;
}

.empty-text {
  font-size: 28rpx;
  color: #AEAEB2;
}
</style>
