<template>
  <view class="page">
    <view :style="{ height: statusBarHeight + 'px' }"></view>
    <view class="nav-bar">
      <view class="nav-back" hover-class="nav-back-active" @tap="goBack">
        <text class="nav-back-icon">‹</text>
      </view>
      <text class="nav-title">我的课程</text>
      <view style="width: 60rpx;"></view>
    </view>

    <view class="tabs">
      <view
        v-for="(tab, index) in tabs"
        :key="index"
        class="tab-item"
        :class="{ 'tab-active': currentTab === index }"
        @tap="switchTab(index)"
      >
        <text class="tab-text" :class="{ 'tab-text-active': currentTab === index }">{{ tab }}</text>
      </view>
    </view>

    <scroll-view class="course-list" scroll-y>
      <view
        v-for="(course, index) in filteredCourses"
        :key="index"
        class="course-card"
      >
        <view class="course-cover" :style="{ background: course.color }">
          <text class="course-cover-icon">{{ course.icon }}</text>
        </view>
        <view class="course-info">
          <text class="course-name">{{ course.name }}</text>
          <text class="course-teacher">讲师：{{ course.teacher }}</text>
          <view class="progress-wrap">
            <view class="progress-bar">
              <view class="progress-fill" :style="{ width: course.progress + '%' }"></view>
            </view>
            <text class="progress-text">{{ course.progress }}%</text>
          </view>
          <text class="course-time">上次学习：{{ course.lastTime }}</text>
        </view>
        <view
          class="course-btn"
          hover-class="course-btn-active"
          @tap="onContinue(course)"
        >
          <text class="course-btn-text">{{ currentTab === 1 ? '再学一次' : '继续学习' }}</text>
        </view>
      </view>

      <view v-if="filteredCourses.length === 0" class="empty">
        <text class="empty-icon">📖</text>
        <text class="empty-text">暂无课程</text>
      </view>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

interface Course {
  type: number
  icon: string
  name: string
  teacher: string
  progress: number
  lastTime: string
  color: string
}

const tabs = ['进行中', '已完成', '已收藏']
const currentTab = ref(0)
const statusBarHeight = ref(0)

onMounted(() => {
  const sysInfo = uni.getSystemInfoSync()
  statusBarHeight.value = sysInfo.statusBarHeight || 0
})

const courses = ref<Course[]>([
  { type: 0, icon: '💻', name: 'Python编程入门', teacher: '张明远', progress: 65, lastTime: '2025-05-07 20:30', color: '#fef3ee' },
  { type: 0, icon: '🎨', name: 'UI设计进阶课', teacher: '李思雨', progress: 40, lastTime: '2025-05-06 19:15', color: '#f0fdf4' },
  { type: 0, icon: '📱', name: '新媒体运营实战', teacher: '王浩然', progress: 20, lastTime: '2025-05-05 21:00', color: '#fefce8' },
  { type: 0, icon: '🗣️', name: '英语口语提升班', teacher: 'Sarah Chen', progress: 55, lastTime: '2025-05-04 18:45', color: '#eff6ff' },
  { type: 1, icon: '📊', name: '数据分析基础', teacher: '陈建国', progress: 100, lastTime: '2025-04-28 20:00', color: '#fef3ee' },
  { type: 1, icon: '📝', name: '简历优化与面试技巧', teacher: '刘晓芳', progress: 100, lastTime: '2025-04-20 19:30', color: '#f0fdf4' },
  { type: 2, icon: '🤖', name: '人工智能导论', teacher: '赵伟', progress: 0, lastTime: '未开始', color: '#fefce8' },
  { type: 2, icon: '📷', name: '短视频剪辑入门', teacher: '周艺', progress: 0, lastTime: '未开始', color: '#eff6ff' }
])

const filteredCourses = computed(() => {
  return courses.value.filter(c => c.type === currentTab.value)
})

const switchTab = (index: number) => {
  currentTab.value = index
}

const goBack = () => {
  uni.navigateBack()
}

const onContinue = (course: Course) => {
  if (course.type === 0 && course.progress < 100) {
    course.progress = Math.min(100, course.progress + 10)
    course.lastTime = new Date().toLocaleString('zh-CN', { year: 'numeric', month: '2-digit', day: '2-digit', hour: '2-digit', minute: '2-digit' }).replace(/\//g, '-')
  }
  uni.showToast({ title: `继续学习：${course.name}`, icon: 'none' })
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: #faf8f5;
}

.nav-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16rpx 32rpx;
  background-color: #faf8f5;
}

.nav-back {
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-back-active {
  opacity: 0.6;
}

.nav-back-icon {
  font-size: 44rpx;
  color: #1a1612;
}

.nav-title {
  font-size: 34rpx;
  font-weight: 700;
  color: #1a1612;
}

.tabs {
  display: flex;
  padding: 16rpx 32rpx;
  background-color: #faf8f5;
}

.tab-item {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20rpx 0;
  border-bottom: 3rpx solid transparent;
}

.tab-active {
  border-bottom-color: #c2410c;
}

.tab-text {
  font-size: 26rpx;
  color: #a89888;
}

.tab-text-active {
  color: #c2410c;
  font-weight: 600;
}

.course-list {
  height: calc(100vh - 180rpx);
  padding: 0 32rpx 32rpx;
}

.course-card {
  display: flex;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 16rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
}

.course-cover {
  width: 120rpx;
  height: 120rpx;
  border-radius: 12rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.course-cover-icon {
  font-size: 48rpx;
}

.course-info {
  flex: 1;
  margin-left: 20rpx;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.course-name {
  font-size: 28rpx;
  font-weight: 600;
  color: #1a1612;
}

.course-teacher {
  font-size: 24rpx;
  color: #a89888;
  margin-top: 6rpx;
}

.progress-wrap {
  display: flex;
  align-items: center;
  margin-top: 12rpx;
}

.progress-bar {
  flex: 1;
  height: 10rpx;
  background: #ede8e0;
  border-radius: 5rpx;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #c2410c;
  border-radius: 5rpx;
}

.progress-text {
  font-size: 22rpx;
  color: #c2410c;
  margin-left: 12rpx;
  flex-shrink: 0;
}

.course-time {
  font-size: 22rpx;
  color: #a89888;
  margin-top: 6rpx;
}

.course-btn {
  flex-shrink: 0;
  align-self: center;
  background: #c2410c;
  border-radius: 8rpx;
  padding: 12rpx 24rpx;
  margin-left: 16rpx;
}

.course-btn-active {
  opacity: 0.6;
}

.course-btn-text {
  font-size: 24rpx;
  color: #ffffff;
  white-space: nowrap;
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
  color: #a89888;
}
</style>
