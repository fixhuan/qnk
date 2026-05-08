<template>
  <view class="page">
    <view class="header">
      <view class="status-bar" :style="{ height: statusBarHeight + 'px' }"></view>
      <view class="header-top">
        <text class="logo-text">青年卡</text>
        <view class="notify-btn" @click="onNotifyClick">
          <text class="notify-icon">🔔</text>
        </view>
      </view>
      <text class="greeting-title">Hi，青年</text>
      <text class="greeting-sub">探索属于你的精彩世界</text>
      <view class="search-bar" @click="onSearchClick">
        <text class="search-icon">🔍</text>
        <text class="search-placeholder">搜索服务、商户、课程...</text>
      </view>
    </view>

    <view class="content">
      <view class="banner-card">
        <swiper
          class="banner-swiper"
          :autoplay="true"
          :interval="4000"
          :duration="500"
          :circular="true"
          :current="bannerCurrent"
          @change="onBannerChange"
        >
          <swiper-item v-for="(item, index) in banners" :key="index">
            <view class="banner-item" :style="{ backgroundColor: item.bg }">
              <view class="banner-text">
                <text class="banner-title">{{ item.title }}</text>
                <text class="banner-desc">{{ item.desc }}</text>
              </view>
              <view class="banner-btn" @click="onBannerClick(item)">
                <text class="banner-btn-text">{{ item.btn }}</text>
              </view>
            </view>
          </swiper-item>
        </swiper>
        <view class="banner-dots">
          <view
            class="banner-dot"
            :class="{ 'banner-dot-active': bannerCurrent === index }"
            v-for="(item, index) in banners"
            :key="index"
          ></view>
        </view>
      </view>

      <view class="section">
        <view class="grid-card">
          <view class="grid-wrap">
            <view
              class="grid-item"
              v-for="(item, index) in modules"
              :key="index"
              @click="onModuleClick(item)"
            >
              <view class="grid-icon-wrap" :style="{ backgroundColor: item.iconBg }">
                <text class="grid-icon">{{ item.icon }}</text>
              </view>
              <text class="grid-name">{{ item.name }}</text>
            </view>
          </view>
        </view>
      </view>

      <view class="section">
        <view class="section-header">
          <text class="section-title">热门推荐</text>
          <text class="section-more" @click="onMoreClick('recommend')">查看更多</text>
        </view>
        <scroll-view class="recommend-scroll" scroll-x :show-scrollbar="false">
          <view class="recommend-list">
            <view
              class="recommend-card"
              v-for="(item, index) in recommends"
              :key="index"
              @click="onRecommendClick(item)"
            >
              <view class="recommend-img" :style="{ backgroundColor: item.bg }">
                <text class="recommend-img-emoji">{{ item.emoji }}</text>
              </view>
              <view class="recommend-info">
                <text class="recommend-title">{{ item.title }}</text>
                <text class="recommend-desc">{{ item.desc }}</text>
                <view class="recommend-tag-wrap">
                  <text class="recommend-tag" :style="{ backgroundColor: item.tagBg, color: item.tagColor }">{{ item.tag }}</text>
                </view>
              </view>
            </view>
          </view>
        </scroll-view>
      </view>

      <view class="section">
        <view class="section-header">
          <text class="section-title">最新活动</text>
          <text class="section-more" @click="onMoreClick('activity')">查看更多</text>
        </view>
        <view class="activity-list">
          <view
            class="activity-item"
            v-for="(item, index) in activities"
            :key="index"
            @click="onActivityClick(item)"
          >
            <view class="activity-icon-box" :style="{ backgroundColor: item.bg }">
              <text class="activity-emoji">{{ item.icon }}</text>
            </view>
            <view class="activity-info">
              <text class="activity-title">{{ item.title }}</text>
              <text class="activity-time">{{ item.time }}</text>
              <text class="activity-people">{{ item.people }}人参与</text>
            </view>
            <view class="activity-arrow">
              <text class="activity-arrow-icon">›</text>
            </view>
          </view>
        </view>
      </view>

      <view class="bottom-space"></view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Banner {
  title: string
  desc: string
  bg: string
  btn: string
  path: string
}

interface Module {
  icon: string
  name: string
  path: string
  iconBg: string
}

interface Recommend {
  title: string
  desc: string
  bg: string
  emoji: string
  tag: string
  tagBg: string
  tagColor: string
  path: string
}

interface Activity {
  title: string
  time: string
  people: number
  bg: string
  icon: string
  path: string
}

const statusBarHeight = ref(0)

uni.getSystemInfoSync &&
  (statusBarHeight.value = uni.getSystemInfoSync().statusBarHeight || 0)

const bannerCurrent = ref(0)

const banners = ref<Banner[]>([
  {
    title: '青春惠享',
    desc: '专属优惠 畅享生活',
    bg: '#c2410c',
    btn: '立即参与',
    path: '/pages/detail/shop'
  },
  {
    title: '夜校报名',
    desc: '技能提升 充实自我',
    bg: '#1a1612',
    btn: '马上报名',
    path: '/pages/detail/school'
  },
  {
    title: '青年公寓',
    desc: '温馨住所 安心安居',
    bg: '#15803d',
    btn: '了解详情',
    path: '/pages/detail/apartment'
  }
])

const modules = ref<Module[]>([
  { icon: '🏪', name: '小店', path: '/pages/detail/shop', iconBg: '#fef3ee' },
  { icon: '📚', name: '夜校', path: '/pages/detail/school', iconBg: '#eff6ff' },
  { icon: '🏠', name: '公寓', path: '/pages/detail/apartment', iconBg: '#f0fdf4' },
  { icon: '💼', name: '求职', path: '/pages/detail/job', iconBg: '#fefce8' },
  { icon: '🚀', name: '创业', path: '/pages/detail/startup', iconBg: '#faf5ff' },
  { icon: '🎉', name: '社交', path: '/pages/detail/social', iconBg: '#fff1f2' },
  { icon: '❤️', name: '志愿', path: '/pages/detail/volunteer', iconBg: '#fef3ee' },
  { icon: '✏️', name: '学习', path: '/pages/detail/learn', iconBg: '#eff6ff' },
  { icon: '🛡️', name: '权益', path: '/pages/detail/rights', iconBg: '#f0fdf4' }
])

const recommends = ref<Recommend[]>([
  {
    title: '创意咖啡工坊',
    desc: '手冲咖啡体验课程',
    bg: '#fef3ee',
    emoji: '☕',
    tag: '热门',
    tagBg: '#fef3ee',
    tagColor: '#c2410c',
    path: '/pages/detail/shop'
  },
  {
    title: 'Python编程入门',
    desc: '零基础到实战项目',
    bg: '#eff6ff',
    emoji: '💻',
    tag: '新课',
    tagBg: '#eff6ff',
    tagColor: '#1d4ed8',
    path: '/pages/detail/learn'
  },
  {
    title: '青年创业沙龙',
    desc: '投资人面对面交流',
    bg: '#faf5ff',
    emoji: '🚀',
    tag: '推荐',
    tagBg: '#fef3ee',
    tagColor: '#c2410c',
    path: '/pages/detail/startup'
  },
  {
    title: '周末户外徒步',
    desc: '结交志同道合伙伴',
    bg: '#f0fdf4',
    emoji: '🥾',
    tag: '社交',
    tagBg: '#f0fdf4',
    tagColor: '#15803d',
    path: '/pages/detail/social'
  }
])

const activities = ref<Activity[]>([
  {
    title: '城市音乐节志愿者招募',
    time: '2026-05-15 14:00',
    people: 128,
    bg: '#fef3ee',
    icon: '🎵',
    path: '/pages/detail/volunteer'
  },
  {
    title: '青年读书分享会',
    time: '2026-05-18 19:00',
    people: 56,
    bg: '#eff6ff',
    icon: '📖',
    path: '/pages/detail/learn'
  },
  {
    title: '创业路演大赛',
    time: '2026-05-20 09:00',
    people: 234,
    bg: '#fefce8',
    icon: '🏆',
    path: '/pages/detail/startup'
  }
])

const onBannerChange = (e: any) => {
  bannerCurrent.value = e.detail.current
}

const onModuleClick = (item: Module) => {
  uni.navigateTo({ url: item.path })
}

const onSearchClick = () => {
  uni.navigateTo({ url: '/pages/detail/shop' })
}

const onNotifyClick = () => {
  uni.navigateTo({ url: '/pages/profile/orders' })
}

const onBannerClick = (item: Banner) => {
  uni.navigateTo({ url: item.path })
}

const onMoreClick = (type: string) => {
  if (type === 'recommend') {
    uni.navigateTo({ url: '/pages/detail/shop' })
  } else {
    uni.navigateTo({ url: '/pages/detail/volunteer' })
  }
}

const onRecommendClick = (item: Recommend) => {
  uni.navigateTo({ url: item.path })
}

const onActivityClick = (item: Activity) => {
  uni.navigateTo({ url: item.path })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #faf8f5;
}

.header {
  background-color: #faf8f5;
  padding: 0 32rpx 32rpx;
}

.status-bar {
  width: 100%;
}

.header-top {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 24rpx;
}

.logo-text {
  font-size: 32rpx;
  font-weight: 700;
  color: #1a1612;
  letter-spacing: 0.05em;
}

.notify-btn {
  width: 72rpx;
  height: 72rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.notify-btn:active {
  opacity: 0.6;
}

.notify-icon {
  font-size: 36rpx;
  color: #6b5e52;
}

.greeting-title {
  font-size: 44rpx;
  font-weight: 700;
  color: #1a1612;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.greeting-sub {
  font-size: 26rpx;
  color: #a89888;
  margin-top: 8rpx;
  margin-bottom: 24rpx;
  display: block;
}

.search-bar {
  display: flex;
  align-items: center;
  height: 80rpx;
  background-color: #ffffff;
  border: 1rpx solid #e8e0d6;
  border-radius: 12rpx;
  padding: 0 24rpx;
}

.search-bar:active {
  background-color: #f5f0ea;
}

.search-icon {
  font-size: 28rpx;
  margin-right: 16rpx;
  color: #a89888;
}

.search-placeholder {
  font-size: 26rpx;
  color: #a89888;
}

.content {
  padding: 0 32rpx;
}

.banner-card {
  background-color: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  overflow: hidden;
  box-shadow: 0 1rpx 4rpx rgba(0, 0, 0, 0.03);
}

.banner-swiper {
  width: 100%;
  height: 280rpx;
}

.banner-item {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 0 40rpx;
}

.banner-text {
  display: flex;
  flex-direction: column;
}

.banner-title {
  font-size: 36rpx;
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 8rpx;
  letter-spacing: -0.02em;
}

.banner-desc {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.8);
  font-weight: 400;
}

.banner-btn {
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 999rpx;
  padding: 12rpx 28rpx;
  border: 1rpx solid rgba(255, 255, 255, 0.3);
  flex-shrink: 0;
}

.banner-btn:active {
  opacity: 0.7;
  background-color: rgba(255, 255, 255, 0.3);
}

.banner-btn-text {
  font-size: 24rpx;
  color: #ffffff;
  font-weight: 600;
}

.banner-dots {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 16rpx 0 20rpx;
  gap: 12rpx;
}

.banner-dot {
  width: 12rpx;
  height: 12rpx;
  border-radius: 50%;
  background-color: #e8e0d6;
  transition: all 0.3s ease;
}

.banner-dot-active {
  width: 28rpx;
  border-radius: 6rpx;
  background-color: #c2410c;
}

.section {
  margin-top: 40rpx;
}

.section-header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 24rpx;
}

.section-title {
  font-size: 32rpx;
  font-weight: 700;
  color: #1a1612;
  letter-spacing: -0.02em;
}

.section-more {
  font-size: 24rpx;
  color: #c2410c;
  font-weight: 400;
}

.section-more:active {
  opacity: 0.6;
}

.grid-card {
  background-color: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  overflow: hidden;
  box-shadow: 0 1rpx 4rpx rgba(0, 0, 0, 0.03);
}

.grid-wrap {
  display: flex;
  flex-wrap: wrap;
  padding: 16rpx 0 8rpx;
}

.grid-item {
  width: 33.333%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20rpx 0 24rpx;
}

.grid-item:active {
  opacity: 0.6;
}

.grid-icon-wrap {
  width: 80rpx;
  height: 80rpx;
  border-radius: 16rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.grid-icon {
  font-size: 40rpx;
}

.grid-name {
  font-size: 24rpx;
  color: #6b5e52;
  margin-top: 12rpx;
  font-weight: 400;
}

.recommend-scroll {
  white-space: nowrap;
}

.recommend-list {
  display: flex;
  flex-direction: row;
  gap: 20rpx;
  padding-bottom: 8rpx;
}

.recommend-card {
  display: inline-flex;
  flex-direction: column;
  width: 280rpx;
  background-color: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  overflow: hidden;
  flex-shrink: 0;
  box-shadow: 0 1rpx 4rpx rgba(0, 0, 0, 0.03);
}

.recommend-card:active {
  opacity: 0.6;
}

.recommend-img {
  width: 100%;
  height: 160rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.recommend-img-emoji {
  font-size: 56rpx;
}

.recommend-info {
  padding: 20rpx 24rpx;
  display: flex;
  flex-direction: column;
}

.recommend-title {
  font-size: 28rpx;
  font-weight: 600;
  color: #1a1612;
  margin-bottom: 8rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.recommend-desc {
  font-size: 22rpx;
  color: #a89888;
  margin-bottom: 16rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.recommend-tag-wrap {
  display: flex;
  flex-direction: row;
}

.recommend-tag {
  font-size: 20rpx;
  padding: 6rpx 16rpx;
  border-radius: 999rpx;
  font-weight: 400;
}

.activity-list {
  display: flex;
  flex-direction: column;
  gap: 16rpx;
}

.activity-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  background-color: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 24rpx;
  box-shadow: 0 1rpx 4rpx rgba(0, 0, 0, 0.03);
}

.activity-item:active {
  opacity: 0.6;
}

.activity-icon-box {
  width: 64rpx;
  height: 64rpx;
  border-radius: 12rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 24rpx;
  flex-shrink: 0;
}

.activity-emoji {
  font-size: 32rpx;
}

.activity-info {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-width: 0;
}

.activity-title {
  font-size: 28rpx;
  font-weight: 600;
  color: #1a1612;
  margin-bottom: 6rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.activity-time {
  font-size: 22rpx;
  color: #a89888;
  margin-bottom: 4rpx;
}

.activity-people {
  font-size: 22rpx;
  color: #c2410c;
  font-weight: 600;
  font-variant-numeric: tabular-nums;
}

.activity-arrow {
  width: 48rpx;
  height: 48rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: 12rpx;
}

.activity-arrow-icon {
  font-size: 36rpx;
  color: #e8e0d6;
  font-weight: 300;
}

.bottom-space {
  height: 40rpx;
}
</style>
