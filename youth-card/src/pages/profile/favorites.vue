<template>
  <view class="page">
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
        <view v-if="currentTab === index" class="tab-indicator"></view>
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
import { ref, computed } from 'vue'

interface Favorite {
  type: number
  icon: string
  name: string
  desc: string
  color: string
}

const tabs = ['商户', '课程', '公寓', '职位']
const currentTab = ref(0)

const favorites = ref<Favorite[]>([
  { type: 0, icon: '☕', name: '星巴克（大学城店）', desc: '青年卡专享8折优惠', color: '#E8F1F5' },
  { type: 0, icon: '🍲', name: '海底捞（高新区店）', desc: '青年卡立减30元', color: '#F8F9FA' },
  { type: 1, icon: '💻', name: 'Python编程入门', desc: '零基础到实战·夜校热门课', color: '#E8F1F5' },
  { type: 1, icon: '🎨', name: 'UI设计进阶课', desc: '从理论到项目实战', color: '#F8F9FA' },
  { type: 2, icon: '🏠', name: '高新区青年公寓', desc: '精装单间·地铁直达·月租1200起', color: '#E8F1F5' },
  { type: 2, icon: '🏢', name: '大学城人才公寓', desc: '一室一厅·配套齐全·月租1800起', color: '#F8F9FA' },
  { type: 3, icon: '💼', name: '前端开发工程师', desc: '字节跳动·15-25K·应届可投', color: '#E8F1F5' },
  { type: 3, icon: '📊', name: '数据分析师', desc: '腾讯·12-20K·实习转正', color: '#F8F9FA' }
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

.fav-list {
  height: calc(100vh - 180rpx);
  padding: 0 32rpx 32rpx;
}

.fav-card {
  display: flex;
  align-items: center;
  background: #FFFFFF;
  border-radius: 16rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
  padding: 24rpx;
  margin-bottom: 20rpx;
}

.fav-thumb {
  width: 100rpx;
  height: 100rpx;
  border-radius: 16rpx;
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
  color: #1C1C1E;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.fav-desc {
  font-size: 24rpx;
  color: #8E8E93;
  margin-top: 8rpx;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.fav-unfav {
  flex-shrink: 0;
  padding: 10rpx 20rpx;
  background: #F8F9FA;
  border-radius: 999rpx;
  margin-left: 16rpx;
}

.fav-unfav-active {
  background: #E5E5EA;
}

.fav-unfav-text {
  font-size: 22rpx;
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
