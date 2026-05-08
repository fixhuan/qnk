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
      <view class="shop-card" v-for="item in filteredShops" :key="item.id">
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
  { id: 1, name: '老街烧烤', desc: '地道炭火烧烤，回味无穷', discount: '8折', distance: '500m', category: '美食', bg: 'linear-gradient(135deg, #FF6B6B 0%, #FF8E8E 100%)', favorited: false },
  { id: 2, name: '鲜味火锅', desc: '正宗川味火锅，麻辣鲜香', discount: '满100减20', distance: '800m', category: '美食', bg: 'linear-gradient(135deg, #E8553A 0%, #FF7B5F 100%)', favorited: false },
  { id: 3, name: '茶百道', desc: '新式茶饮，鲜果现做', discount: '买一送一', distance: '300m', category: '饮品', bg: 'linear-gradient(135deg, #4FC7F7 0%, #88D8FF 100%)', favorited: true },
  { id: 4, name: '瑞幸咖啡', desc: '精品咖啡，每日新鲜烘焙', discount: '9折', distance: '450m', category: '饮品', bg: 'linear-gradient(135deg, #8B6F4E 0%, #C4A676 100%)', favorited: false },
  { id: 5, name: '优品生活馆', desc: '品质生活，一站式购物', discount: '满200减50', distance: '1.2km', category: '生活', bg: 'linear-gradient(135deg, #4F6EF7 0%, #7B9AFF 100%)', favorited: false },
  { id: 6, name: '星空KTV', desc: '欢唱之夜，释放自我', discount: '7折', distance: '1.5km', category: '娱乐', bg: 'linear-gradient(135deg, #9B59B6 0%, #C39BD3 100%)', favorited: true },
  { id: 7, name: '力美健身', desc: '专业教练指导，科学健身', discount: '年卡8折', distance: '900m', category: '健身', bg: 'linear-gradient(135deg, #2ECC71 0%, #58D68D 100%)', favorited: false },
  { id: 8, name: '型格造型', desc: '潮流发型设计，焕然一新', discount: '首次5折', distance: '600m', category: '美发', bg: 'linear-gradient(135deg, #F7854F 0%, #FFB088 100%)', favorited: false }
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
  background-color: #F5F6FA;
  display: flex;
  flex-direction: column;
}

.search-bar {
  padding: 20rpx 24rpx;
  background-color: #FFFFFF;
}

.search-input-wrap {
  display: flex;
  align-items: center;
  background-color: #F5F6FA;
  border-radius: 36rpx;
  padding: 16rpx 24rpx;
}

.search-icon {
  font-size: 28rpx;
  margin-right: 12rpx;
}

.search-input {
  flex: 1;
  font-size: 28rpx;
  color: #333333;
}

.search-placeholder {
  color: #999999;
  font-size: 28rpx;
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

.shop-card {
  display: flex;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
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
  color: #333333;
}

.shop-desc {
  font-size: 24rpx;
  color: #999999;
  margin-top: 8rpx;
}

.shop-tags {
  display: flex;
  margin-top: 12rpx;
}

.discount-tag {
  font-size: 22rpx;
  color: #4F6EF7;
  background-color: rgba(79, 110, 247, 0.1);
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
  color: #999999;
}

.fav-btn {
  padding: 4rpx 8rpx;
}

.fav-icon {
  font-size: 32rpx;
}
</style>
