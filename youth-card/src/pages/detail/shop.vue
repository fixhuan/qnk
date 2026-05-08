<template>
  <view class="page">
    <view class="orb orb-1"></view>
    <view class="orb orb-2"></view>

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
              <text class="fav-icon">{{ item.favorited ? '❤️' : '🤍' }}</text>
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
  { id: 1, name: '老街烧烤', desc: '地道炭火烧烤，回味无穷', discount: '8折', distance: '500m', category: '美食', bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)', favorited: false },
  { id: 2, name: '鲜味火锅', desc: '正宗川味火锅，麻辣鲜香', discount: '满100减20', distance: '800m', category: '美食', bg: 'linear-gradient(135deg, #fa709a 0%, #fee140 100%)', favorited: false },
  { id: 3, name: '茶百道', desc: '新式茶饮，鲜果现做', discount: '买一送一', distance: '300m', category: '饮品', bg: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)', favorited: true },
  { id: 4, name: '瑞幸咖啡', desc: '精品咖啡，每日新鲜烘焙', discount: '9折', distance: '450m', category: '饮品', bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)', favorited: false },
  { id: 5, name: '优品生活馆', desc: '品质生活，一站式购物', discount: '满200减50', distance: '1.2km', category: '生活', bg: 'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)', favorited: false },
  { id: 6, name: '星空KTV', desc: '欢唱之夜，释放自我', discount: '7折', distance: '1.5km', category: '娱乐', bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)', favorited: true },
  { id: 7, name: '力美健身', desc: '专业教练指导，科学健身', discount: '年卡8折', distance: '900m', category: '健身', bg: 'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)', favorited: false },
  { id: 8, name: '型格造型', desc: '潮流发型设计，焕然一新', discount: '首次5折', distance: '600m', category: '美发', bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)', favorited: false }
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
  background-color: #0f0f2d;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
}

.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80rpx);
  z-index: 0;
}

.orb-1 {
  width: 400rpx;
  height: 400rpx;
  background: #667eea;
  opacity: 0.15;
  top: -100rpx;
  right: -100rpx;
}

.orb-2 {
  width: 300rpx;
  height: 300rpx;
  background: #f093fb;
  opacity: 0.1;
  top: 200rpx;
  left: -80rpx;
}

.search-bar {
  padding: 20rpx 24rpx;
  position: relative;
  z-index: 1;
}

.search-input-wrap {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  border-radius: 40rpx;
  padding: 16rpx 24rpx;
  backdrop-filter: blur(20px);
}

.search-icon {
  font-size: 28rpx;
  margin-right: 12rpx;
}

.search-input {
  flex: 1;
  font-size: 28rpx;
  color: #FFFFFF;
}

.search-placeholder {
  color: rgba(255, 255, 255, 0.4);
  font-size: 28rpx;
}

.category-scroll {
  white-space: nowrap;
  padding: 16rpx 24rpx;
  position: relative;
  z-index: 1;
}

.category-tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12rpx 32rpx;
  border-radius: 32rpx;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  margin-right: 16rpx;
  transition: all 0.3s;
}

.category-tag.active {
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-color: transparent;
  box-shadow: 0 4rpx 16rpx rgba(102, 126, 234, 0.4);
}

.category-text {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.6);
}

.category-text.active-text {
  color: #FFFFFF;
  font-weight: 600;
}

.list-scroll {
  flex: 1;
  padding: 20rpx 24rpx;
  position: relative;
  z-index: 1;
}

.shop-card {
  display: flex;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  border-radius: 24rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  backdrop-filter: blur(20px);
  transition: all 0.3s;
}

.card-hover {
  transform: scale(0.95);
  opacity: 0.85;
}

.shop-img {
  width: 160rpx;
  height: 160rpx;
  border-radius: 16rpx;
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
  font-weight: 700;
  color: #FFFFFF;
}

.shop-desc {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.6);
  margin-top: 8rpx;
}

.shop-tags {
  display: flex;
  margin-top: 12rpx;
}

.discount-tag {
  font-size: 22rpx;
  color: #FFFFFF;
  background: linear-gradient(135deg, #667eea, #764ba2);
  padding: 4rpx 16rpx;
  border-radius: 16rpx;
}

.shop-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 12rpx;
}

.shop-distance {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.4);
}

.fav-btn {
  padding: 4rpx 8rpx;
}

.fav-icon {
  font-size: 32rpx;
}
</style>
