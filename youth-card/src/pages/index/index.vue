<template>
  <view class="page">
    <view class="hero">
      <view class="hero-bg"></view>
      <view class="aurora-orb aurora-orb-1"></view>
      <view class="aurora-orb aurora-orb-2"></view>
      <view class="aurora-orb aurora-orb-3"></view>
      <view class="hero-content">
        <view class="status-bar" :style="{ height: statusBarHeight + 'px' }"></view>
        <view class="hero-top">
          <text class="logo-text">青年卡</text>
          <view class="notify-btn" @click="onNotifyClick">
            <text class="notify-icon">🔔</text>
          </view>
        </view>
        <view class="greeting">
          <text class="greeting-title">Hi，青年 👋</text>
          <text class="greeting-sub">探索属于你的精彩世界</text>
        </view>
        <view class="search-bar" @click="onSearchClick">
          <text class="search-icon">🔍</text>
          <text class="search-placeholder">搜索服务、商户、课程...</text>
        </view>
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
            <view class="banner-item" :style="{ background: item.bg }">
              <view class="banner-orb banner-orb-1" :style="{ background: item.orbColor1 }"></view>
              <view class="banner-orb banner-orb-2" :style="{ background: item.orbColor2 }"></view>
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
              :style="{ '--item-gradient': item.gradient }"
            >
              <view class="grid-icon-wrap" :style="{ background: item.gradient }">
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
              <view class="recommend-img" :style="{ background: item.bg }">
                <view class="recommend-img-orb" :style="{ background: item.orbColor }"></view>
              </view>
              <view class="recommend-info">
                <text class="recommend-title">{{ item.title }}</text>
                <text class="recommend-desc">{{ item.desc }}</text>
                <view class="recommend-tag-wrap">
                  <text class="recommend-tag" :style="{ background: item.tagBg }">{{ item.tag }}</text>
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
            <view class="activity-img" :style="{ background: item.bg }">
              <text class="activity-icon">{{ item.icon }}</text>
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
  orbColor1: string
  orbColor2: string
  path: string
}

interface Module {
  icon: string
  name: string
  path: string
  gradient: string
}

interface Recommend {
  title: string
  desc: string
  bg: string
  tag: string
  tagBg: string
  orbColor: string
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
    bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
    btn: '立即参与',
    orbColor1: 'rgba(240,147,251,0.3)',
    orbColor2: 'rgba(79,172,254,0.25)',
    path: '/pages/detail/shop'
  },
  {
    title: '夜校报名',
    desc: '技能提升 充实自我',
    bg: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)',
    btn: '马上报名',
    orbColor1: 'rgba(67,233,123,0.3)',
    orbColor2: 'rgba(102,126,234,0.25)',
    path: '/pages/detail/school'
  },
  {
    title: '青年公寓',
    desc: '温馨住所 安心安居',
    bg: 'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)',
    btn: '了解详情',
    orbColor1: 'rgba(79,172,254,0.3)',
    orbColor2: 'rgba(250,112,154,0.25)',
    path: '/pages/detail/apartment'
  }
])

const modules = ref<Module[]>([
  { icon: '🏪', name: '小店', path: '/pages/detail/shop', gradient: 'linear-gradient(135deg, #f5576c, #f093fb)' },
  { icon: '📚', name: '夜校', path: '/pages/detail/school', gradient: 'linear-gradient(135deg, #4facfe, #00f2fe)' },
  { icon: '🏠', name: '公寓', path: '/pages/detail/apartment', gradient: 'linear-gradient(135deg, #43e97b, #38f9d7)' },
  { icon: '💼', name: '求职', path: '/pages/detail/job', gradient: 'linear-gradient(135deg, #fa709a, #fee140)' },
  { icon: '🚀', name: '创业', path: '/pages/detail/startup', gradient: 'linear-gradient(135deg, #667eea, #764ba2)' },
  { icon: '🎉', name: '社交', path: '/pages/detail/social', gradient: 'linear-gradient(135deg, #f093fb, #f5576c)' },
  { icon: '❤️', name: '志愿', path: '/pages/detail/volunteer', gradient: 'linear-gradient(135deg, #fee140, #fa709a)' },
  { icon: '✏️', name: '学习', path: '/pages/detail/learn', gradient: 'linear-gradient(135deg, #7c5cfc, #9b85fd)' },
  { icon: '🛡️', name: '权益', path: '/pages/detail/rights', gradient: 'linear-gradient(135deg, #38f9d7, #43e97b)' }
])

const recommends = ref<Recommend[]>([
  {
    title: '创意咖啡工坊',
    desc: '手冲咖啡体验课程',
    bg: 'linear-gradient(135deg, #f5576c, #f093fb)',
    tag: '热门',
    tagBg: 'linear-gradient(135deg, #f5576c, #f093fb)',
    orbColor: 'rgba(255,255,255,0.15)',
    path: '/pages/detail/shop'
  },
  {
    title: 'Python编程入门',
    desc: '零基础到实战项目',
    bg: 'linear-gradient(135deg, #4facfe, #00f2fe)',
    tag: '新课',
    tagBg: 'linear-gradient(135deg, #4facfe, #00f2fe)',
    orbColor: 'rgba(255,255,255,0.15)',
    path: '/pages/detail/learn'
  },
  {
    title: '青年创业沙龙',
    desc: '投资人面对面交流',
    bg: 'linear-gradient(135deg, #667eea, #764ba2)',
    tag: '推荐',
    tagBg: 'linear-gradient(135deg, #667eea, #764ba2)',
    orbColor: 'rgba(255,255,255,0.15)',
    path: '/pages/detail/startup'
  },
  {
    title: '周末户外徒步',
    desc: '结交志同道合伙伴',
    bg: 'linear-gradient(135deg, #43e97b, #38f9d7)',
    tag: '社交',
    tagBg: 'linear-gradient(135deg, #43e97b, #38f9d7)',
    orbColor: 'rgba(255,255,255,0.15)',
    path: '/pages/detail/social'
  }
])

const activities = ref<Activity[]>([
  {
    title: '城市音乐节志愿者招募',
    time: '2026-05-15 14:00',
    people: 128,
    bg: 'linear-gradient(135deg, #f093fb, #f5576c)',
    icon: '🎵',
    path: '/pages/detail/volunteer'
  },
  {
    title: '青年读书分享会',
    time: '2026-05-18 19:00',
    people: 56,
    bg: 'linear-gradient(135deg, #667eea, #764ba2)',
    icon: '📖',
    path: '/pages/detail/learn'
  },
  {
    title: '创业路演大赛',
    time: '2026-05-20 09:00',
    people: 234,
    bg: 'linear-gradient(135deg, #fa709a, #fee140)',
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
  background-color: #0f0f2d;
}

.hero {
  position: relative;
  overflow: hidden;
  padding-bottom: 40rpx;
}

.hero-bg {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(180deg, #0c0c1d 0%, #1a1a3e 100%);
  z-index: 0;
}

.aurora-orb {
  position: absolute;
  border-radius: 50%;
  z-index: 0;
}

.aurora-orb-1 {
  width: 400rpx;
  height: 400rpx;
  top: -80rpx;
  right: -60rpx;
  background: radial-gradient(circle, rgba(240,147,251,0.2) 0%, rgba(245,87,108,0.08) 60%, transparent 100%);
  filter: blur(80rpx);
}

.aurora-orb-2 {
  width: 500rpx;
  height: 500rpx;
  top: 60rpx;
  left: -120rpx;
  background: radial-gradient(circle, rgba(79,172,254,0.18) 0%, rgba(0,242,254,0.06) 60%, transparent 100%);
  filter: blur(80rpx);
}

.aurora-orb-3 {
  width: 350rpx;
  height: 350rpx;
  bottom: -40rpx;
  left: 50%;
  transform: translateX(-50%);
  background: radial-gradient(circle, rgba(67,233,123,0.15) 0%, rgba(56,249,215,0.05) 60%, transparent 100%);
  filter: blur(80rpx);
}

.hero-content {
  position: relative;
  z-index: 1;
  padding: 0 32rpx;
}

.status-bar {
  width: 100%;
}

.hero-top {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 32rpx;
}

.logo-text {
  font-size: 36rpx;
  font-weight: 800;
  background: linear-gradient(135deg, #667eea 0%, #f093fb 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.notify-btn {
  width: 72rpx;
  height: 72rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255,255,255,0.08);
  border: 1rpx solid rgba(255,255,255,0.1);
  border-radius: 50%;
  backdrop-filter: blur(20px);
}

.notify-btn:active {
  transform: scale(0.92);
  background: rgba(255,255,255,0.12);
}

.notify-icon {
  font-size: 32rpx;
}

.greeting {
  display: flex;
  flex-direction: column;
  margin-bottom: 32rpx;
}

.greeting-title {
  font-size: 48rpx;
  font-weight: 800;
  color: #FFFFFF;
  margin-bottom: 8rpx;
  letter-spacing: 1rpx;
}

.greeting-sub {
  font-size: 26rpx;
  color: rgba(255,255,255,0.6);
}

.search-bar {
  display: flex;
  align-items: center;
  height: 80rpx;
  background: rgba(255,255,255,0.06);
  border: 1rpx solid rgba(255,255,255,0.1);
  border-radius: 40rpx;
  padding: 0 28rpx;
  backdrop-filter: blur(20px);
}

.search-bar:active {
  background: rgba(255,255,255,0.1);
  transform: scale(0.98);
}

.search-icon {
  font-size: 28rpx;
  margin-right: 16rpx;
}

.search-placeholder {
  font-size: 26rpx;
  color: rgba(255,255,255,0.4);
}

.content {
  position: relative;
  z-index: 2;
  padding: 0 32rpx;
  margin-top: -16rpx;
}

.banner-card {
  background: rgba(255,255,255,0.06);
  border: 1rpx solid rgba(255,255,255,0.1);
  border-radius: 28rpx;
  overflow: hidden;
  backdrop-filter: blur(20px);
  box-shadow: 0 8rpx 48rpx rgba(0,0,0,0.5);
}

.banner-swiper {
  width: 100%;
  height: 320rpx;
}

.banner-item {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 0 40rpx;
  position: relative;
  overflow: hidden;
}

.banner-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(60rpx);
}

.banner-orb-1 {
  width: 260rpx;
  height: 260rpx;
  top: -60rpx;
  right: -40rpx;
  opacity: 0.6;
}

.banner-orb-2 {
  width: 200rpx;
  height: 200rpx;
  bottom: -40rpx;
  left: 40rpx;
  opacity: 0.5;
}

.banner-text {
  display: flex;
  flex-direction: column;
  position: relative;
  z-index: 1;
}

.banner-title {
  font-size: 40rpx;
  font-weight: 800;
  color: #FFFFFF;
  margin-bottom: 8rpx;
  letter-spacing: 2rpx;
}

.banner-desc {
  font-size: 24rpx;
  color: rgba(255,255,255,0.8);
}

.banner-btn {
  background: rgba(255,255,255,0.25);
  border-radius: 40rpx;
  padding: 14rpx 32rpx;
  backdrop-filter: blur(10px);
  border: 1rpx solid rgba(255,255,255,0.2);
  position: relative;
  z-index: 1;
}

.banner-btn:active {
  transform: scale(0.95);
  background: rgba(255,255,255,0.35);
}

.banner-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
  font-weight: 600;
}

.banner-dots {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 20rpx 0 24rpx;
  gap: 12rpx;
}

.banner-dot {
  width: 12rpx;
  height: 12rpx;
  border-radius: 50%;
  background: rgba(255,255,255,0.2);
  transition: all 0.3s ease;
}

.banner-dot-active {
  width: 32rpx;
  border-radius: 6rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
}

.section {
  margin-top: 36rpx;
}

.section-header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 24rpx;
}

.section-title {
  font-size: 34rpx;
  font-weight: 800;
  color: #FFFFFF;
}

.section-more {
  font-size: 24rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-weight: 500;
}

.section-more:active {
  opacity: 0.7;
}

.grid-card {
  background: rgba(255,255,255,0.06);
  border: 1rpx solid rgba(255,255,255,0.1);
  border-radius: 28rpx;
  backdrop-filter: blur(20px);
  box-shadow: 0 4rpx 24rpx rgba(0,0,0,0.4);
  overflow: hidden;
}

.grid-wrap {
  display: flex;
  flex-wrap: wrap;
  padding: 24rpx 0 8rpx;
}

.grid-item {
  width: 33.333%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20rpx 0 28rpx;
  transition: transform 0.2s ease;
}

.grid-item:active {
  transform: scale(0.92);
}

.grid-icon-wrap {
  width: 100rpx;
  height: 100rpx;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 14rpx;
  box-shadow: 0 4rpx 20rpx rgba(0,0,0,0.3);
}

.grid-icon {
  font-size: 44rpx;
}

.grid-name {
  font-size: 24rpx;
  color: rgba(255,255,255,0.85);
  font-weight: 500;
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
  background: rgba(255,255,255,0.06);
  border: 1rpx solid rgba(255,255,255,0.1);
  border-radius: 20rpx;
  overflow: hidden;
  backdrop-filter: blur(20px);
  box-shadow: 0 4rpx 24rpx rgba(0,0,0,0.4);
  flex-shrink: 0;
  transition: transform 0.2s ease;
}

.recommend-card:active {
  transform: scale(0.95);
}

.recommend-img {
  width: 100%;
  height: 180rpx;
  position: relative;
  overflow: hidden;
}

.recommend-img-orb {
  position: absolute;
  width: 160rpx;
  height: 160rpx;
  border-radius: 50%;
  top: -40rpx;
  right: -30rpx;
  filter: blur(40rpx);
}

.recommend-info {
  padding: 20rpx 24rpx;
  display: flex;
  flex-direction: column;
}

.recommend-title {
  font-size: 28rpx;
  font-weight: 700;
  color: #FFFFFF;
  margin-bottom: 8rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.recommend-desc {
  font-size: 22rpx;
  color: rgba(255,255,255,0.5);
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
  padding: 6rpx 20rpx;
  border-radius: 20rpx;
  font-weight: 600;
  color: #FFFFFF;
}

.activity-list {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}

.activity-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  background: rgba(255,255,255,0.06);
  border: 1rpx solid rgba(255,255,255,0.1);
  border-radius: 20rpx;
  padding: 28rpx 24rpx;
  backdrop-filter: blur(20px);
  box-shadow: 0 4rpx 24rpx rgba(0,0,0,0.4);
  transition: transform 0.2s ease;
}

.activity-item:active {
  transform: scale(0.97);
}

.activity-img {
  width: 96rpx;
  height: 96rpx;
  border-radius: 20rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 24rpx;
  flex-shrink: 0;
}

.activity-icon {
  font-size: 40rpx;
}

.activity-info {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-width: 0;
}

.activity-title {
  font-size: 28rpx;
  font-weight: 700;
  color: #FFFFFF;
  margin-bottom: 8rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.activity-time {
  font-size: 22rpx;
  color: rgba(255,255,255,0.4);
  margin-bottom: 4rpx;
}

.activity-people {
  font-size: 22rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-weight: 600;
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
  color: rgba(255,255,255,0.2);
  font-weight: 300;
}

.bottom-space {
  height: 32rpx;
}
</style>
