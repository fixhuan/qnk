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
  background-color: #0f0f2d;
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

.tabs {
  display: flex;
  padding: 16rpx 24rpx;
  gap: 12rpx;
}

.tab-item {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16rpx 0;
  border-radius: 32rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
}

.tab-active {
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-color: transparent;
  box-shadow: 0 4rpx 20rpx rgba(102, 126, 234, 0.4);
}

.tab-text {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.5);
}

.tab-text-active {
  color: #FFFFFF;
  font-weight: 600;
}

.order-list {
  height: calc(100vh - 180rpx);
  padding: 0 24rpx 24rpx;
}

.order-card {
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 24rpx;
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
  color: #FFFFFF;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.order-status {
  font-size: 24rpx;
  padding: 6rpx 20rpx;
  border-radius: 20rpx;
  flex-shrink: 0;
  margin-left: 16rpx;
}

.status-pending {
  color: #fee140;
  background: linear-gradient(135deg, rgba(250, 112, 154, 0.2), rgba(254, 225, 64, 0.2));
  border: 1rpx solid rgba(254, 225, 64, 0.3);
}

.status-using {
  color: #00f2fe;
  background: linear-gradient(135deg, rgba(79, 172, 254, 0.2), rgba(0, 242, 254, 0.2));
  border: 1rpx solid rgba(0, 242, 254, 0.3);
}

.status-done {
  color: #43e97b;
  background: linear-gradient(135deg, rgba(67, 233, 123, 0.2), rgba(56, 249, 215, 0.2));
  border: 1rpx solid rgba(67, 233, 123, 0.3);
}

.status-cancel {
  color: rgba(255, 255, 255, 0.4);
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
}

.order-body {
  display: flex;
  flex-direction: column;
  gap: 8rpx;
  padding-bottom: 20rpx;
  border-bottom: 1rpx solid rgba(255, 255, 255, 0.06);
}

.order-no {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.4);
}

.order-time {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.4);
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
  background: linear-gradient(135deg, #f093fb, #f5576c);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.order-actions {
  display: flex;
  gap: 16rpx;
}

.action-btn {
  padding: 10rpx 28rpx;
  border-radius: 28rpx;
}

.action-btn-primary {
  background: linear-gradient(135deg, #667eea, #764ba2);
  box-shadow: 0 4rpx 16rpx rgba(102, 126, 234, 0.3);
}

.action-btn-default {
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.15);
}

.action-btn-active {
  transform: scale(0.95);
  opacity: 0.8;
}

.action-btn-text-primary {
  font-size: 24rpx;
  color: #FFFFFF;
}

.action-btn-text-default {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.6);
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
  color: rgba(255, 255, 255, 0.3);
}
</style>
