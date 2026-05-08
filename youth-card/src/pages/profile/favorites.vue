<template>
  <view class="page">
    <view class="tabs">
      <view
        v-for="(tab, index) in tabs"
        :key="index"
        class="tab-item"
        :class="{ 'tab-active': currentTab === index }"
        @tap="switchTab(index)"
      >
        <text class="tab-text" :class="{ 'tab-text-active': currentTab === index }">{{ tab }}</text>
        <view v-if="currentTab === index" class="tab-line"></view>
      </view>
    </view>

    <scroll-view class="fav-list" scroll-y>
      <view
        v-for="(item, index) in filteredFavorites"
        :key="index"
        class="fav-card"
      >
        <view class="fav-thumb" :style="{ background: item.gradient }">
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
  gradient: string
}

const tabs = ['商户', '课程', '公寓', '职位']
const currentTab = ref(0)

const favorites = ref<Favorite[]>([
  { type: 0, icon: '☕', name: '星巴克（大学城店）', desc: '青年卡专享8折优惠', gradient: 'linear-gradient(135deg, #667eea, #764ba2)' },
  { type: 0, icon: '🍲', name: '海底捞（高新区店）', desc: '青年卡立减30元', gradient: 'linear-gradient(135deg, #f093fb, #f5576c)' },
  { type: 1, icon: '💻', name: 'Python编程入门', desc: '零基础到实战·夜校热门课', gradient: 'linear-gradient(135deg, #4facfe, #00f2fe)' },
  { type: 1, icon: '🎨', name: 'UI设计进阶课', desc: '从理论到项目实战', gradient: 'linear-gradient(135deg, #43e97b, #38f9d7)' },
  { type: 2, icon: '🏠', name: '高新区青年公寓', desc: '精装单间·地铁直达·月租1200起', gradient: 'linear-gradient(135deg, #fa709a, #fee140)' },
  { type: 2, icon: '🏢', name: '大学城人才公寓', desc: '一室一厅·配套齐全·月租1800起', gradient: 'linear-gradient(135deg, #a18cd1, #fbc2eb)' },
  { type: 3, icon: '💼', name: '前端开发工程师', desc: '字节跳动·15-25K·应届可投', gradient: 'linear-gradient(135deg, #fccb90, #d57eeb)' },
  { type: 3, icon: '📊', name: '数据分析师', desc: '腾讯·12-20K·实习转正', gradient: 'linear-gradient(135deg, #96fbc4, #f9f586)' }
])

const filteredFavorites = computed(() => {
  return favorites.value.filter(f => f.type === currentTab.value)
})

const switchTab = (index: number) => {
  currentTab.value = index
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
  background-color: #F5F6FA;
}

.tabs {
  display: flex;
  background-color: #FFFFFF;
  padding: 0 8rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.04);
}

.tab-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 24rpx 0 16rpx;
  position: relative;
}

.tab-text {
  font-size: 28rpx;
  color: #666666;
}

.tab-text-active {
  color: #4F6EF7;
  font-weight: 600;
}

.tab-line {
  width: 40rpx;
  height: 6rpx;
  background-color: #4F6EF7;
  border-radius: 3rpx;
  margin-top: 8rpx;
}

.fav-list {
  height: calc(100vh - 80rpx);
  padding: 24rpx;
}

.fav-card {
  display: flex;
  align-items: center;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
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
  color: #333333;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.fav-desc {
  font-size: 24rpx;
  color: #999999;
  margin-top: 8rpx;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.fav-unfav {
  flex-shrink: 0;
  padding: 10rpx 20rpx;
  border: 1rpx solid #CCCCCC;
  border-radius: 24rpx;
  margin-left: 16rpx;
}

.fav-unfav-active {
  background-color: #F5F6FA;
  transform: scale(0.96);
}

.fav-unfav-text {
  font-size: 22rpx;
  color: #666666;
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
  color: #999999;
}
</style>
