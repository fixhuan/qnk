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
      <view class="apt-card" v-for="item in filteredApartments" :key="item.id">
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
  { id: 1, name: '青春公寓', address: '城东区青年路88号', type: '单间', price: 800, facilities: ['WiFi', '空调', '热水器'], area: '城东', bg: 'linear-gradient(135deg, #4F6EF7 0%, #7B9AFF 100%)', favorited: false },
  { id: 2, name: '梦想家公寓', address: '城东区创业大道126号', type: '一室一厅', price: 1500, facilities: ['WiFi', '空调', '洗衣机', '冰箱'], area: '城东', bg: 'linear-gradient(135deg, #F7854F 0%, #FFB088 100%)', favorited: true },
  { id: 3, name: '安居青年社', address: '城西区科技路56号', type: '单间', price: 900, facilities: ['WiFi', '空调', '热水器'], area: '城西', bg: 'linear-gradient(135deg, #2ECC71 0%, #58D68D 100%)', favorited: false },
  { id: 4, name: '悦居公寓', address: '城西区大学城旁', type: '一室一厅', price: 1800, facilities: ['WiFi', '空调', '洗衣机', '冰箱', '厨房'], area: '城西', bg: 'linear-gradient(135deg, #9B59B6 0%, #C39BD3 100%)', favorited: false },
  { id: 5, name: '城南花园公寓', address: '城南新区商业街', type: '两室一厅', price: 2500, facilities: ['WiFi', '空调', '洗衣机', '冰箱', '厨房', '阳台'], area: '城南', bg: 'linear-gradient(135deg, #E8553A 0%, #FF7B5F 100%)', favorited: false },
  { id: 6, name: '星河公寓', address: '城南高铁站旁', type: '单间', price: 1200, facilities: ['WiFi', '空调', '热水器'], area: '城南', bg: 'linear-gradient(135deg, #4FC7F7 0%, #88D8FF 100%)', favorited: true },
  { id: 7, name: '北岸青年公寓', address: '城北产业园旁', type: '一室一厅', price: 1600, facilities: ['WiFi', '空调', '洗衣机', '冰箱'], area: '城北', bg: 'linear-gradient(135deg, #FF6B6B 0%, #FF8E8E 100%)', favorited: false },
  { id: 8, name: '优享公寓', address: '城北体育中心旁', type: '两室一厅', price: 2200, facilities: ['WiFi', '空调', '洗衣机', '冰箱', '厨房', '阳台'], area: '城北', bg: 'linear-gradient(135deg, #8B6F4E 0%, #C4A676 100%)', favorited: false }
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

.filter-bar {
  background-color: #FFFFFF;
  padding: 12rpx 24rpx 16rpx;
  border-bottom: 1rpx solid #F0F0F0;
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
  border-radius: 28rpx;
  background-color: #F0F2F5;
  margin-right: 12rpx;
}

.filter-tag.active {
  background-color: #4F6EF7;
}

.filter-text {
  font-size: 24rpx;
  color: #666666;
}

.filter-text.active-text {
  color: #FFFFFF;
  font-weight: 600;
}

.list-scroll {
  flex: 1;
  padding: 20rpx 24rpx;
}

.apt-card {
  display: flex;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.apt-img {
  width: 200rpx;
  height: 200rpx;
  border-radius: 16rpx;
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
  font-weight: 700;
  color: #333333;
}

.apt-address {
  font-size: 22rpx;
  color: #999999;
  margin-top: 8rpx;
}

.apt-tags {
  display: flex;
  margin-top: 10rpx;
}

.apt-type-tag {
  font-size: 22rpx;
  color: #4F6EF7;
  background-color: rgba(79, 110, 247, 0.1);
  padding: 4rpx 16rpx;
  border-radius: 16rpx;
}

.apt-facilities {
  display: flex;
  flex-wrap: wrap;
  gap: 8rpx;
  margin-top: 10rpx;
}

.facility-tag {
  font-size: 20rpx;
  color: #666666;
  background-color: #F5F6FA;
  padding: 4rpx 12rpx;
  border-radius: 12rpx;
}

.apt-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 12rpx;
}

.apt-price {
  font-size: 36rpx;
  font-weight: 700;
  color: #FF6B6B;
}

.apt-price-unit {
  font-size: 22rpx;
  font-weight: 400;
  color: #999999;
}

.fav-btn {
  padding: 4rpx 8rpx;
}

.fav-icon {
  font-size: 32rpx;
}
</style>
