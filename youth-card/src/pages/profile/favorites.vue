<template>
  <view class="page">
    <view :style="{ height: statusBarHeight + 'px' }"></view>
    <view class="nav-bar">
      <view class="nav-back" hover-class="nav-back-active" @tap="goBack">
        <text class="nav-back-icon">‹</text>
      </view>
      <text class="nav-title">我的收藏</text>
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

    <scroll-view class="fav-list" scroll-y>
      <view
        v-for="(item, index) in filteredFavorites"
        :key="index"
        class="fav-card"
      >
        <view class="fav-thumb" :style="{ background: item.color }">
          <text class="fav-thumb-icon">{{ item.icon }}</text>
        </view>
        <view class="fav-info">
          <text class="fav-name">{{ item.name }}</text>
          <text class="fav-desc">{{ item.desc }}</text>
        </view>
        <view
          class="fav-unfav"
          hover-class="fav-unfav-active"
          @tap="onUnfav(item, index)"
        >
          <text class="fav-unfav-text">取消收藏</text>
        </view>
      </view>

      <view v-if="filteredFavorites.length === 0" class="empty">
        <text class="empty-icon">⭐</text>
        <text class="empty-text">暂无收藏</text>
      </view>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

interface Favorite {
  type: number
  icon: string
  name: string
  desc: string
  color: string
}

const tabs = ['商户', '课程', '公寓', '职位']
const currentTab = ref(0)
const statusBarHeight = ref(0)

onMounted(() => {
  const sysInfo = uni.getSystemInfoSync()
  statusBarHeight.value = sysInfo.statusBarHeight || 0
})

const favorites = ref<Favorite[]>([
  { type: 0, icon: '☕', name: '星巴克（大学城店）', desc: '青年卡专享8折优惠', color: '#fef3ee' },
  { type: 0, icon: '🍲', name: '海底捞（高新区店）', desc: '青年卡立减30元', color: '#fefce8' },
  { type: 1, icon: '💻', name: 'Python编程入门', desc: '零基础到实战·夜校热门课', color: '#eff6ff' },
  { type: 1, icon: '🎨', name: 'UI设计进阶课', desc: '从理论到项目实战', color: '#f0fdf4' },
  { type: 2, icon: '🏠', name: '高新区青年公寓', desc: '精装单间·地铁直达·月租1200起', color: '#fef3ee' },
  { type: 2, icon: '🏢', name: '大学城人才公寓', desc: '一室一厅·配套齐全·月租1800起', color: '#fefce8' },
  { type: 3, icon: '💼', name: '前端开发工程师', desc: '字节跳动·15-25K·应届可投', color: '#eff6ff' },
  { type: 3, icon: '📊', name: '数据分析师', desc: '腾讯·12-20K·实习转正', color: '#f0fdf4' }
])

const filteredFavorites = computed(() => {
  return favorites.value.filter(f => f.type === currentTab.value)
})

const switchTab = (index: number) => {
  currentTab.value = index
}

const goBack = () => {
  uni.navigateBack()
}

const onUnfav = (item: Favorite, index: number) => {
  uni.showModal({
    title: '提示',
    content: `确定取消收藏"${item.name}"吗？`,
    success: (res) => {
      if (res.confirm) {
        const globalIdx = favorites.value.indexOf(item)
        if (globalIdx > -1) favorites.value.splice(globalIdx, 1)
        uni.showToast({ title: '已取消收藏', icon: 'none' })
      }
    }
  })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #faf8f5;
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

.tabs {
  display: flex;
  padding: 16rpx 32rpx;
  background-color: #faf8f5;
}

.tab-item {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20rpx 0;
  border-bottom: 3rpx solid transparent;
}

.tab-active {
  border-bottom-color: #c2410c;
}

.tab-text {
  font-size: 26rpx;
  color: #a89888;
}

.tab-text-active {
  color: #c2410c;
  font-weight: 600;
}

.fav-list {
  height: calc(100vh - 180rpx);
  padding: 0 32rpx 32rpx;
}

.fav-card {
  display: flex;
  align-items: center;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
}

.fav-thumb {
  width: 100rpx;
  height: 100rpx;
  border-radius: 12rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.fav-thumb-icon {
  font-size: 44rpx;
}

.fav-info {
  flex: 1;
  margin-left: 20rpx;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.fav-name {
  font-size: 28rpx;
  font-weight: 600;
  color: #1a1612;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.fav-desc {
  font-size: 24rpx;
  color: #a89888;
  margin-top: 8rpx;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.fav-unfav {
  flex-shrink: 0;
  padding: 10rpx 20rpx;
  background: #f5f0ea;
  border-radius: 999rpx;
  margin-left: 16rpx;
}

.fav-unfav-active {
  opacity: 0.6;
}

.fav-unfav-text {
  font-size: 22rpx;
  color: #6b5e52;
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
  color: #a89888;
}
</style>
