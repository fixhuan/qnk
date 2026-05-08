<template>
  <view class="page">
    <view class="search-bar">
      <view class="search-input-wrap">
        <text class="search-icon">🔍</text>
        <input class="search-input" placeholder="搜索商户" placeholder-class="search-placeholder" v-model="keyword" />
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
      <view class="shop-card" v-for="item in filteredShops" :key="item.id" hover-class="card-hover">
        <view class="shop-img" :style="{ background: item.bg }"></view>
        <view class="shop-info">
          <text class="shop-name">{{ item.name }}</text>
          <text class="shop-desc">{{ item.desc }}</text>
          <view class="shop-tags">
            <text class="discount-tag">{{ item.discount }}</text>
          </view>
          <view class="shop-bottom">
            <text class="shop-distance">{{ item.distance }}</text>
            <view class="fav-btn" @click="item.favorited = !item.favorited">
              <text class="fav-icon" :class="{ favorited: item.favorited }">{{ item.favorited ? '❤' : '♡' }}</text>
            </view>
          </view>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Shop {
  id: number
  name: string
  desc: string
  discount: string
  distance: string
  category: string
  bg: string
  favorited: boolean
}

const keyword = ref('')
const currentCategory = ref('全部')
const categories = ['全部', '美食', '饮品', '生活', '娱乐', '健身', '美发']

const shops = ref<Shop[]>([
  { id: 1, name: '老街烧烤', desc: '地道炭火烧烤，回味无穷', discount: '8折', distance: '500m', category: '美食', bg: '#fef3ee', favorited: false },
  { id: 2, name: '鲜味火锅', desc: '正宗川味火锅，麻辣鲜香', discount: '满100减20', distance: '800m', category: '美食', bg: '#fefce8', favorited: false },
  { id: 3, name: '茶百道', desc: '新式茶饮，鲜果现做', discount: '买一送一', distance: '300m', category: '饮品', bg: '#f0fdf4', favorited: true },
  { id: 4, name: '瑞幸咖啡', desc: '精品咖啡，每日新鲜烘焙', discount: '9折', distance: '450m', category: '饮品', bg: '#eff6ff', favorited: false },
  { id: 5, name: '优品生活馆', desc: '品质生活，一站式购物', discount: '满200减50', distance: '1.2km', category: '生活', bg: '#fef3ee', favorited: false },
  { id: 6, name: '星空KTV', desc: '欢唱之夜，释放自我', discount: '7折', distance: '1.5km', category: '娱乐', bg: '#eff6ff', favorited: true },
  { id: 7, name: '力美健身', desc: '专业教练指导，科学健身', discount: '年卡8折', distance: '900m', category: '健身', bg: '#f0fdf4', favorited: false },
  { id: 8, name: '型格造型', desc: '潮流发型设计，焕然一新', discount: '首次5折', distance: '600m', category: '美发', bg: '#fefce8', favorited: false }
])

const filteredShops = computed(() => {
  return shops.value.filter(item => {
    const matchCategory = currentCategory.value === '全部' || item.category === currentCategory.value
    const matchKeyword = !keyword.value || item.name.includes(keyword.value) || item.desc.includes(keyword.value)
    return matchCategory && matchKeyword
  })
})
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

.shop-card {
  display: flex;
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

.shop-img {
  width: 160rpx;
  height: 160rpx;
  border-radius: 12rpx;
  flex-shrink: 0;
}

.shop-info {
  flex: 1;
  margin-left: 24rpx;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.shop-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1a1612;
}

.shop-desc {
  font-size: 24rpx;
  color: #6b5e52;
  margin-top: 8rpx;
}

.shop-tags {
  display: flex;
  margin-top: 12rpx;
}

.discount-tag {
  font-size: 22rpx;
  color: #c2410c;
  background: #fef3ee;
  padding: 4rpx 16rpx;
  border-radius: 999rpx;
}

.shop-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 12rpx;
}

.shop-distance {
  font-size: 22rpx;
  color: #a89888;
}

.fav-btn {
  padding: 4rpx 8rpx;
}

.fav-icon {
  font-size: 32rpx;
  color: #e8e0d6;
}

.fav-icon.favorited {
  color: #c2410c;
}
</style>
