<template>
  <view class="page">
    <view class="search-bar">
      <view class="search-input-wrap">
        <text class="search-icon">🔍</text>
        <input class="search-input" placeholder="搜索课程、学习计划、资料" placeholder-class="search-placeholder" v-model="keyword" />
      </view>
    </view>

    <view class="stats-bar">
      <view class="stat-card" hover-class="card-hover">
        <text class="stat-value">2.5h</text>
        <text class="stat-label">今日学习时长</text>
      </view>
      <view class="stat-card" hover-class="card-hover">
        <text class="stat-value">15天</text>
        <text class="stat-label">连续打卡</text>
      </view>
      <view class="stat-card" hover-class="card-hover">
        <text class="stat-value">8</text>
        <text class="stat-label">已完成课程</text>
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
      <template v-if="currentCategory === '推荐课程'">
        <view class="course-card" v-for="item in filteredCourses" :key="item.id" hover-class="card-hover" @click="onCourseClick(item)">
          <view class="course-cover" :style="{ background: item.bg }"></view>
          <view class="course-info">
            <text class="course-name">{{ item.name }}</text>
            <text class="course-teacher">讲师：{{ item.teacher }}</text>
            <text class="course-people">{{ item.learners }}人学习</text>
            <view class="course-progress-wrap">
              <view class="progress-bar">
                <view class="progress-fill" :style="{ width: item.progress + '%' }"></view>
              </view>
              <text class="progress-text">{{ item.progress }}%</text>
            </view>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '学习计划'">
        <view class="plan-card" v-for="item in filteredPlans" :key="item.id" hover-class="card-hover">
          <view class="plan-header">
            <text class="plan-name">{{ item.name }}</text>
            <text class="plan-days">已坚持{{ item.days }}天</text>
          </view>
          <view class="plan-progress-wrap">
            <view class="progress-bar">
              <view class="progress-fill" :style="{ width: item.progress + '%' }"></view>
            </view>
            <text class="progress-text">{{ item.progress }}%</text>
          </view>
          <view class="plan-btn" @click="onContinueLearn(item)">
            <text class="plan-btn-text">继续学习</text>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '资料库'">
        <view class="resource-card" v-for="item in filteredResources" :key="item.id" hover-class="card-hover">
          <view class="resource-icon" :style="{ background: item.bg }">
            <text class="resource-icon-text">{{ item.typeIcon }}</text>
          </view>
          <view class="resource-info">
            <text class="resource-name">{{ item.name }}</text>
            <view class="resource-meta">
              <text class="resource-type">{{ item.type }}</text>
              <text class="resource-size">{{ item.size }}</text>
            </view>
          </view>
          <view class="download-btn" :class="{ downloaded: item.downloaded }" @click="onDownload(item)">
            <text class="download-btn-text" :class="{ downloaded: item.downloaded }">{{ item.downloaded ? '已下载' : '下载' }}</text>
          </view>
        </view>
      </template>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Course {
  id: number
  name: string
  teacher: string
  learners: number
  progress: number
  bg: string
}

interface Plan {
  id: number
  name: string
  progress: number
  days: number
}

interface Resource {
  id: number
  name: string
  type: string
  typeIcon: string
  size: string
  bg: string
  downloaded: boolean
}

const currentCategory = ref('推荐课程')
const categories = ['推荐课程', '学习计划', '资料库']
const keyword = ref('')

const courses = ref<Course[]>([
  { id: 1, name: 'Vue3从入门到实战', teacher: '张老师', learners: 2386, progress: 68, bg: '#fef3ee' },
  { id: 2, name: 'TypeScript高级编程', teacher: '李老师', learners: 1562, progress: 35, bg: '#f0fdf4' },
  { id: 3, name: 'React Hooks深度解析', teacher: '王老师', learners: 1890, progress: 0, bg: '#eff6ff' },
  { id: 4, name: 'Node.js后端开发', teacher: '赵老师', learners: 1245, progress: 92, bg: '#fefce8' },
  { id: 5, name: 'CSS动画与交互设计', teacher: '刘老师', learners: 978, progress: 15, bg: '#fef3ee' },
  { id: 6, name: '微信小程序开发实战', teacher: '陈老师', learners: 3120, progress: 50, bg: '#f0fdf4' }
])

const plans = ref<Plan[]>([
  { id: 1, name: '前端工程师30天进阶', progress: 68, days: 20 },
  { id: 2, name: '每日算法打卡', progress: 45, days: 15 },
  { id: 3, name: '英语口语提升计划', progress: 30, days: 9 },
  { id: 4, name: 'Python数据分析入门', progress: 80, days: 24 },
  { id: 5, name: '产品设计思维训练', progress: 20, days: 6 }
])

const resources = ref<Resource[]>([
  { id: 1, name: 'Vue3官方文档精编', type: 'PDF', typeIcon: 'PDF', size: '12.5MB', bg: '#fef3ee', downloaded: false },
  { id: 2, name: 'TypeScript速查手册', type: 'PDF', typeIcon: 'PDF', size: '3.2MB', bg: '#fef3ee', downloaded: true },
  { id: 3, name: '前端面试题集2026', type: 'DOC', typeIcon: 'DOC', size: '8.7MB', bg: '#eff6ff', downloaded: false },
  { id: 4, name: 'CSS布局实战视频', type: '视频', typeIcon: 'MP4', size: '256MB', bg: '#f0fdf4', downloaded: false },
  { id: 5, name: 'React源码解析', type: 'PDF', typeIcon: 'PDF', size: '15.3MB', bg: '#fef3ee', downloaded: false },
  { id: 6, name: 'Node.js最佳实践', type: 'EPUB', typeIcon: 'EPUB', size: '6.8MB', bg: '#fefce8', downloaded: true }
])

const filteredCourses = computed(() => {
  return courses.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value) || item.teacher.includes(keyword.value)
  })
})

const filteredPlans = computed(() => {
  return plans.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value)
  })
})

const filteredResources = computed(() => {
  return resources.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value) || item.type.includes(keyword.value)
  })
})

const onCourseClick = (item: Course) => {
  uni.showToast({ title: `${item.name} · ${item.teacher}`, icon: 'none' })
}

const onContinueLearn = (item: Plan) => {
  uni.showToast({ title: '继续学习中', icon: 'success' })
}

const onDownload = (item: Resource) => {
  if (item.downloaded) return
  item.downloaded = true
  uni.showToast({ title: '下载成功', icon: 'success' })
}
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

.stats-bar {
  display: flex;
  padding: 24rpx;
  gap: 16rpx;
}

.stat-card {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 24rpx 0;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.card-hover {
  opacity: 0.6;
}

.stat-value {
  font-size: 36rpx;
  font-weight: 700;
  color: #c2410c;
}

.stat-label {
  font-size: 22rpx;
  color: #6b5e52;
  margin-top: 8rpx;
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

.course-card {
  display: flex;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.course-cover {
  width: 180rpx;
  height: 180rpx;
  border-radius: 12rpx;
  flex-shrink: 0;
}

.course-info {
  flex: 1;
  margin-left: 24rpx;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.course-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #1a1612;
}

.course-teacher {
  font-size: 24rpx;
  color: #6b5e52;
  margin-top: 8rpx;
}

.course-people {
  font-size: 22rpx;
  color: #a89888;
  margin-top: 6rpx;
}

.course-progress-wrap {
  display: flex;
  align-items: center;
  margin-top: 12rpx;
}

.progress-bar {
  flex: 1;
  height: 8rpx;
  background: #ede8e0;
  border-radius: 4rpx;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #c2410c;
  border-radius: 4rpx;
}

.progress-text {
  font-size: 20rpx;
  color: #a89888;
  margin-left: 12rpx;
}

.plan-card {
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.plan-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.plan-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #1a1612;
}

.plan-days {
  font-size: 24rpx;
  color: #c2410c;
  background: #fef3ee;
  padding: 6rpx 16rpx;
  border-radius: 999rpx;
}

.plan-progress-wrap {
  display: flex;
  align-items: center;
  margin-top: 20rpx;
}

.plan-btn {
  margin-top: 20rpx;
  padding: 14rpx 0;
  background: #c2410c;
  border-radius: 8rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.plan-btn-text {
  font-size: 26rpx;
  color: #ffffff;
}

.resource-card {
  display: flex;
  align-items: center;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.resource-icon {
  width: 72rpx;
  height: 72rpx;
  border-radius: 12rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.resource-icon-text {
  font-size: 22rpx;
  font-weight: 700;
  color: #c2410c;
}

.resource-info {
  flex: 1;
  margin-left: 20rpx;
}

.resource-name {
  font-size: 28rpx;
  font-weight: 600;
  color: #1a1612;
}

.resource-meta {
  display: flex;
  align-items: center;
  gap: 16rpx;
  margin-top: 8rpx;
}

.resource-type {
  font-size: 22rpx;
  color: #1d4ed8;
  background: #eff6ff;
  padding: 2rpx 10rpx;
  border-radius: 999rpx;
}

.resource-size {
  font-size: 22rpx;
  color: #a89888;
}

.download-btn {
  padding: 10rpx 24rpx;
  background: #c2410c;
  border-radius: 8rpx;
  flex-shrink: 0;
}

.download-btn.downloaded {
  background: #f5f0ea;
}

.download-btn-text {
  font-size: 24rpx;
  color: #ffffff;
}

.download-btn-text.downloaded {
  color: #a89888;
}
</style>
