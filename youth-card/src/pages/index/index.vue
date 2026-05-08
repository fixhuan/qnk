<template>
  <view class="page">
    <view class="banner-wrap">
      <swiper
        class="banner-swiper"
        :autoplay="true"
        :interval="3000"
        :duration="500"
        :circular="true"
        indicator-dots
        indicator-color="rgba(255,255,255,0.4)"
        indicator-active-color="#ffffff"
      >
        <swiper-item v-for="(item, index) in banners" :key="index">
          <view class="banner-item" :style="{ background: item.bg }">
            <text class="banner-title">{{ item.title }}</text>
            <text class="banner-desc">{{ item.desc }}</text>
          </view>
        </swiper-item>
      </swiper>
    </view>

    <view class="grid-wrap">
      <view
        class="grid-item"
        v-for="(item, index) in modules"
        :key="index"
        @click="onModuleClick(item)"
      >
        <text class="grid-icon">{{ item.icon }}</text>
        <text class="grid-name">{{ item.name }}</text>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Banner {
  title: string
  desc: string
  bg: string
}

interface Module {
  icon: string
  name: string
  path: string
}

const banners = ref<Banner[]>([
  {
    title: '青春惠享',
    desc: '专属优惠 畅享生活',
    bg: 'linear-gradient(135deg, #4F6EF7 0%, #7B9AFF 100%)'
  },
  {
    title: '夜校报名',
    desc: '技能提升 充实自我',
    bg: 'linear-gradient(135deg, #F7854F 0%, #FFB088 100%)'
  },
  {
    title: '青年公寓',
    desc: '温馨住所 安心安居',
    bg: 'linear-gradient(135deg, #4FC7F7 0%, #88D8FF 100%)'
  }
])

const modules = ref<Module[]>([
  { icon: '🏪', name: '小店', path: '/pages/detail/shop' },
  { icon: '📚', name: '夜校', path: '/pages/detail/school' },
  { icon: '🏠', name: '公寓', path: '/pages/detail/apartment' },
  { icon: '💼', name: '求职', path: '/pages/detail/job' },
  { icon: '🚀', name: '创业', path: '/pages/detail/startup' },
  { icon: '🎉', name: '社交', path: '/pages/detail/social' },
  { icon: '❤️', name: '志愿', path: '/pages/detail/volunteer' },
  { icon: '✏️', name: '学习', path: '/pages/detail/learn' },
  { icon: '🛡️', name: '权益', path: '/pages/detail/rights' }
])

const onModuleClick = (item: Module) => {
  uni.navigateTo({ url: item.path })
}
</script>

<style>
.page {
  min-height: 100vh;
  background-color: #F5F6FA;
  padding: 24rpx;
  box-sizing: border-box;
}

.banner-wrap {
  border-radius: 24rpx;
  overflow: hidden;
  box-shadow: 0 8rpx 24rpx rgba(79, 110, 247, 0.2);
}

.banner-swiper {
  width: 100%;
  height: 300rpx;
}

.banner-item {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 24rpx;
}

.banner-title {
  font-size: 44rpx;
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 12rpx;
}

.banner-desc {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.85);
}

.grid-wrap {
  display: flex;
  flex-wrap: wrap;
  margin-top: 32rpx;
  gap: 20rpx;
}

.grid-item {
  width: calc((100% - 40rpx) / 3);
  height: 200rpx;
  background-color: #ffffff;
  border-radius: 20rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
  transition: all 0.2s;
}

.grid-item:active {
  transform: scale(0.95);
  background-color: #f0f2ff;
}

.grid-icon {
  font-size: 40rpx;
  margin-bottom: 16rpx;
}

.grid-name {
  font-size: 24rpx;
  color: #333333;
}
</style>
