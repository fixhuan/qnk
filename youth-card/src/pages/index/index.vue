<template>
  <view class="page">
    <view class="header">
      <view class="header-bg">
        <view class="header-wave"></view>
      </view>
      <view class="header-content">
        <view class="status-bar" :style="{ height: statusBarHeight + 'px' }"></view>
        <view class="greeting">
          <text class="greeting-title">Hi，青年！</text>
          <text class="greeting-sub">发现你的精彩生活</text>
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
          indicator-dots
          indicator-color="rgba(255,255,255,0.4)"
          indicator-active-color="#ffffff"
        >
          <swiper-item v-for="(item, index) in banners" :key="index">
            <view class="banner-item" :style="{ background: item.bg }">
              <view class="banner-text">
                <text class="banner-title">{{ item.title }}</text>
                <text class="banner-desc">{{ item.desc }}</text>
              </view>
              <view class="banner-btn">
                <text class="banner-btn-text">{{ item.btn }}</text>
              </view>
            </view>
          </swiper-item>
        </swiper>
      </view>

      <view class="section">
        <view class="grid-wrap">
          <view
            class="grid-item"
            v-for="(item, index) in modules"
            :key="index"
            @click="onModuleClick(item)"
          >
            <view class="grid-icon-wrap" :style="{ background: item.gradient }">
              <text class="grid-icon">{{ item.icon }}</text>
            </view>
            <text class="grid-name">{{ item.name }}</text>
          </view>
        </view>
      </view>

      <view class="section">
        <view class="section-header">
          <text class="section-title">热门推荐</text>
          <text class="section-more" @click="onMoreClick('recommend')">查看更多 ></text>
        </view>
        <scroll-view class="recommend-scroll" scroll-x :show-scrollbar="false">
          <view class="recommend-list">
            <view
              class="recommend-card"
              v-for="(item, index) in recommends"
              :key="index"
              @click="onRecommendClick(item)"
            >
              <view class="recommend-img" :style="{ background: item.bg }"></view>
              <view class="recommend-info">
                <text class="recommend-title">{{ item.title }}</text>
                <text class="recommend-desc">{{ item.desc }}</text>
                <view class="recommend-tag-wrap">
                  <text class="recommend-tag" :style="{ background: item.tagBg, color: item.tagColor }">{{ item.tag }}</text>
                </view>
              </view>
            </view>
          </view>
        </scroll-view>
      </view>

      <view class="section">
        <view class="section-header">
          <text class="section-title">最新活动</text>
          <text class="section-more" @click="onMoreClick('activity')">查看更多 ></text>
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
          </view>
        </view>
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
  btn: string
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

const banners = ref<Banner[]>([
  {
    title: '青春惠享',
    desc: '专属优惠 畅享生活',
    bg: 'linear-gradient(135deg, #4F6EF7 0%, #7B93FA 100%)',
    btn: '立即参与'
  },
  {
    title: '夜校报名',
    desc: '技能提升 充实自我',
    bg: 'linear-gradient(135deg, #FF9F43 0%, #FFBE76 100%)',
    btn: '马上报名'
  },
  {
    title: '青年公寓',
    desc: '温馨住所 安心安居',
    bg: 'linear-gradient(135deg, #2ED573 0%, #7BED9F 100%)',
    btn: '了解详情'
  }
])

const modules = ref<Module[]>([
  { icon: '🏪', name: '小店', path: '/pages/detail/shop', gradient: 'linear-gradient(135deg, #FF6B6B, #FF8E8E)' },
  { icon: '📚', name: '夜校', path: '/pages/detail/school', gradient: 'linear-gradient(135deg, #45AAF2, #74C0FC)' },
  { icon: '🏠', name: '公寓', path: '/pages/detail/apartment', gradient: 'linear-gradient(135deg, #2ED573, #7BED9F)' },
  { icon: '💼', name: '求职', path: '/pages/detail/job', gradient: 'linear-gradient(135deg, #FF9F43, #FFBE76)' },
  { icon: '🚀', name: '创业', path: '/pages/detail/startup', gradient: 'linear-gradient(135deg, #A55EEA, #C49BFF)' },
  { icon: '🎉', name: '社交', path: '/pages/detail/social', gradient: 'linear-gradient(135deg, #FF6B81, #FF9FAE)' },
  { icon: '❤️', name: '志愿', path: '/pages/detail/volunteer', gradient: 'linear-gradient(135deg, #FD9644, #FDCB6E)' },
  { icon: '✏️', name: '学习', path: '/pages/detail/learn', gradient: 'linear-gradient(135deg, #4F6EF7, #7B93FA)' },
  { icon: '🛡️', name: '权益', path: '/pages/detail/rights', gradient: 'linear-gradient(135deg, #26DE81, #7BED9F)' }
])

const recommends = ref<Recommend[]>([
  {
    title: '创意咖啡工坊',
    desc: '手冲咖啡体验课程',
    bg: 'linear-gradient(135deg, #FF6B6B, #FF8E8E)',
    tag: '热门',
    tagBg: 'rgba(255,107,107,0.1)',
    tagColor: '#FF6B6B',
    path: '/pages/detail/shop'
  },
  {
    title: 'Python编程入门',
    desc: '零基础到实战项目',
    bg: 'linear-gradient(135deg, #45AAF2, #74C0FC)',
    tag: '新课',
    tagBg: 'rgba(69,170,242,0.1)',
    tagColor: '#45AAF2',
    path: '/pages/detail/learn'
  },
  {
    title: '青年创业沙龙',
    desc: '投资人面对面交流',
    bg: 'linear-gradient(135deg, #A55EEA, #C49BFF)',
    tag: '推荐',
    tagBg: 'rgba(165,94,234,0.1)',
    tagColor: '#A55EEA',
    path: '/pages/detail/startup'
  },
  {
    title: '周末户外徒步',
    desc: '结交志同道合伙伴',
    bg: 'linear-gradient(135deg, #2ED573, #7BED9F)',
    tag: '社交',
    tagBg: 'rgba(46,213,115,0.1)',
    tagColor: '#2ED573',
    path: '/pages/detail/social'
  }
])

const activities = ref<Activity[]>([
  {
    title: '城市音乐节志愿者招募',
    time: '2026-05-15 14:00',
    people: 128,
    bg: 'linear-gradient(135deg, #FF6B6B, #FF8E8E)',
    icon: '🎵',
    path: '/pages/detail/volunteer'
  },
  {
    title: '青年读书分享会',
    time: '2026-05-18 19:00',
    people: 56,
    bg: 'linear-gradient(135deg, #4F6EF7, #7B93FA)',
    icon: '📖',
    path: '/pages/detail/learn'
  },
  {
    title: '创业路演大赛',
    time: '2026-05-20 09:00',
    people: 234,
    bg: 'linear-gradient(135deg, #FF9F43, #FFBE76)',
    icon: '🏆',
    path: '/pages/detail/startup'
  }
])

const onModuleClick = (item: Module) => {
  uni.navigateTo({ url: item.path })
}

const onSearchClick = () => {
  uni.navigateTo({ url: '/pages/detail/shop' })
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
  background-color: #F5F6FA;
}

.header {
  position: relative;
  overflow: hidden;
}

.header-bg {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, #4F6EF7 0%, #7B93FA 100%);
  z-index: 0;
}

.header-wave {
  position: absolute;
  bottom: -2rpx;
  left: -10%;
  right: -10%;
  height: 60rpx;
  background: #F5F6FA;
  border-radius: 50% 50% 0 0;
}

.header-content {
  position: relative;
  z-index: 1;
  padding: 0 32rpx 48rpx;
}

.status-bar {
  width: 100%;
}

.greeting {
  display: flex;
  flex-direction: column;
  margin-bottom: 28rpx;
}

.greeting-title {
  font-size: 44rpx;
  font-weight: 700;
  color: #FFFFFF;
  margin-bottom: 8rpx;
}

.greeting-sub {
  font-size: 28rpx;
  color: rgba(255, 255, 255, 0.8);
}

.search-bar {
  display: flex;
  align-items: center;
  height: 72rpx;
  background: rgba(255, 255, 255, 0.25);
  border-radius: 36rpx;
  padding: 0 28rpx;
  backdrop-filter: blur(10px);
}

.search-icon {
  font-size: 28rpx;
  margin-right: 16rpx;
}

.search-placeholder {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.7);
}

.content {
  position: relative;
  z-index: 2;
  padding: 0 32rpx 32rpx;
  margin-top: -20rpx;
}

.banner-card {
  background: #FFFFFF;
  border-radius: 28rpx;
  overflow: hidden;
  box-shadow: 0 8rpx 32rpx rgba(79, 110, 247, 0.12);
}

.banner-swiper {
  width: 100%;
  height: 300rpx;
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
  font-size: 40rpx;
  font-weight: 700;
  color: #FFFFFF;
  margin-bottom: 8rpx;
}

.banner-desc {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.85);
}

.banner-btn {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 32rpx;
  padding: 12rpx 28rpx;
  backdrop-filter: blur(10px);
}

.banner-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
  font-weight: 500;
}

.section {
  margin-top: 32rpx;
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
  font-weight: 700;
  color: #1A1A2E;
}

.section-more {
  font-size: 24rpx;
  color: #999999;
}

.grid-wrap {
  display: flex;
  flex-wrap: wrap;
  background: #FFFFFF;
  border-radius: 28rpx;
  padding: 24rpx 0;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.04);
}

.grid-item {
  width: 33.333%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20rpx 0;
}

.grid-item:active {
  opacity: 0.7;
}

.grid-icon-wrap {
  width: 96rpx;
  height: 96rpx;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 12rpx;
}

.grid-icon {
  font-size: 40rpx;
}

.grid-name {
  font-size: 24rpx;
  color: #555770;
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
  background: #FFFFFF;
  border-radius: 20rpx;
  overflow: hidden;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.06);
  flex-shrink: 0;
}

.recommend-card:active {
  opacity: 0.85;
}

.recommend-img {
  width: 100%;
  height: 180rpx;
}

.recommend-info {
  padding: 20rpx 24rpx;
  display: flex;
  flex-direction: column;
}

.recommend-title {
  font-size: 28rpx;
  font-weight: 600;
  color: #1A1A2E;
  margin-bottom: 8rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.recommend-desc {
  font-size: 22rpx;
  color: #999999;
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
  padding: 4rpx 16rpx;
  border-radius: 8rpx;
  font-weight: 500;
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
  background: #FFFFFF;
  border-radius: 20rpx;
  padding: 24rpx;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.04);
}

.activity-item:active {
  opacity: 0.85;
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
  font-weight: 600;
  color: #1A1A2E;
  margin-bottom: 8rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.activity-time {
  font-size: 22rpx;
  color: #999999;
  margin-bottom: 4rpx;
}

.activity-people {
  font-size: 22rpx;
  color: #4F6EF7;
}
</style>
