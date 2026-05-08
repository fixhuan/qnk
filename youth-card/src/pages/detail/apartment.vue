<template>
  <view class="page">
    <view class="search-bar">
      <view class="search-input-wrap">
        <text class="search-icon">🔍</text>
        <input class="search-input" placeholder="搜索公寓" placeholder-class="search-placeholder" v-model="keyword" />
      </view>
    </view>

    <view class="filter-bar">
      <scroll-view class="filter-scroll" scroll-x :show-scrollbar="false">
        <view
          class="filter-tag"
          :class="{ active: currentArea === item }"
          v-for="item in areas"
          :key="item"
          @click="currentArea = item"
        >
          <text class="filter-text" :class="{ 'active-text': currentArea === item }">{{ item }}</text>
        </view>
      </scroll-view>
      <scroll-view class="filter-scroll" scroll-x :show-scrollbar="false">
        <view
          class="filter-tag"
          :class="{ active: currentPrice === item }"
          v-for="item in prices"
          :key="item"
          @click="currentPrice = item"
        >
          <text class="filter-text" :class="{ 'active-text': currentPrice === item }">{{ item }}</text>
        </view>
      </scroll-view>
    </view>

    <scroll-view class="list-scroll" scroll-y>
      <view class="apt-card" v-for="item in filteredApartments" :key="item.id" hover-class="card-hover">
        <view class="apt-img" :style="{ background: item.bg }"></view>
        <view class="apt-info">
          <text class="apt-name">{{ item.name }}</text>
          <text class="apt-address">{{ item.address }}</text>
          <view class="apt-tags">
            <text class="apt-type-tag">{{ item.type }}</text>
          </view>
          <view class="apt-facilities">
            <text class="facility-tag" v-for="f in item.facilities" :key="f">{{ f }}</text>
          </view>
          <view class="apt-bottom">
            <text class="apt-price">¥{{ item.price }}<text class="apt-price-unit">/月</text></text>
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

interface Apartment {
  id: number
  name: string
  address: string
  type: string
  price: number
  facilities: string[]
  area: string
  bg: string
  favorited: boolean
}

const keyword = ref('')
const currentArea = ref('全部')
const currentPrice = ref('不限')
const areas = ['全部', '城东', '城西', '城南', '城北']
const prices = ['不限', '1000以下', '1000-2000', '2000以上']

const apartments = ref<Apartment[]>([
  { id: 1, name: '青春公寓', address: '城东区青年路88号', type: '单间', price: 800, facilities: ['WiFi', '空调', '热水器'], area: '城东', bg: '#fef3ee', favorited: false },
  { id: 2, name: '梦想家公寓', address: '城东区创业大道126号', type: '一室一厅', price: 1500, facilities: ['WiFi', '空调', '洗衣机', '冰箱'], area: '城东', bg: '#f0fdf4', favorited: true },
  { id: 3, name: '安居青年社', address: '城西区科技路56号', type: '单间', price: 900, facilities: ['WiFi', '空调', '热水器'], area: '城西', bg: '#eff6ff', favorited: false },
  { id: 4, name: '悦居公寓', address: '城西区大学城旁', type: '一室一厅', price: 1800, facilities: ['WiFi', '空调', '洗衣机', '冰箱', '厨房'], area: '城西', bg: '#fefce8', favorited: false },
  { id: 5, name: '城南花园公寓', address: '城南新区商业街', type: '两室一厅', price: 2500, facilities: ['WiFi', '空调', '洗衣机', '冰箱', '厨房', '阳台'], area: '城南', bg: '#fef3ee', favorited: false },
  { id: 6, name: '星河公寓', address: '城南高铁站旁', type: '单间', price: 1200, facilities: ['WiFi', '空调', '热水器'], area: '城南', bg: '#f0fdf4', favorited: true },
  { id: 7, name: '北岸青年公寓', address: '城北产业园旁', type: '一室一厅', price: 1600, facilities: ['WiFi', '空调', '洗衣机', '冰箱'], area: '城北', bg: '#eff6ff', favorited: false },
  { id: 8, name: '优享公寓', address: '城北体育中心旁', type: '两室一厅', price: 2200, facilities: ['WiFi', '空调', '洗衣机', '冰箱', '厨房', '阳台'], area: '城北', bg: '#fefce8', favorited: false }
])

const filteredApartments = computed(() => {
  return apartments.value.filter(item => {
    const matchArea = currentArea.value === '全部' || item.area === currentArea.value
    const matchKeyword = !keyword.value || item.name.includes(keyword.value) || item.address.includes(keyword.value)
    let matchPrice = true
    if (currentPrice.value === '1000以下') matchPrice = item.price < 1000
    else if (currentPrice.value === '1000-2000') matchPrice = item.price >= 1000 && item.price <= 2000
    else if (currentPrice.value === '2000以上') matchPrice = item.price > 2000
    return matchArea && matchKeyword && matchPrice
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

.filter-bar {
  padding: 12rpx 24rpx 16rpx;
}

.filter-scroll {
  white-space: nowrap;
  margin-bottom: 8rpx;
}

.filter-tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 10rpx 28rpx;
  border-radius: 999rpx;
  background: #f5f0ea;
  margin-right: 12rpx;
}

.filter-tag.active {
  background: #c2410c;
}

.filter-text {
  font-size: 24rpx;
  color: #6b5e52;
}

.filter-text.active-text {
  color: #ffffff;
  font-weight: 600;
}

.list-scroll {
  flex: 1;
  padding: 20rpx 24rpx;
}

.apt-card {
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

.apt-img {
  width: 200rpx;
  height: 200rpx;
  border-radius: 12rpx;
  flex-shrink: 0;
}

.apt-info {
  flex: 1;
  margin-left: 24rpx;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.apt-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #1a1612;
}

.apt-address {
  font-size: 22rpx;
  color: #a89888;
  margin-top: 8rpx;
}

.apt-tags {
  display: flex;
  margin-top: 10rpx;
}

.apt-type-tag {
  font-size: 22rpx;
  color: #1d4ed8;
  background: #eff6ff;
  padding: 4rpx 16rpx;
  border-radius: 999rpx;
}

.apt-facilities {
  display: flex;
  flex-wrap: wrap;
  gap: 8rpx;
  margin-top: 10rpx;
}

.facility-tag {
  font-size: 20rpx;
  color: #6b5e52;
  background: #f5f0ea;
  padding: 4rpx 12rpx;
  border-radius: 999rpx;
}

.apt-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 12rpx;
}

.apt-price {
  font-size: 36rpx;
  font-weight: 600;
  color: #c2410c;
}

.apt-price-unit {
  font-size: 22rpx;
  font-weight: 400;
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
