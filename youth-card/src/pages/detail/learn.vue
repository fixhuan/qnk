<template>
  <view class="page">
    <view class="orb orb-1"></view>
    <view class="orb orb-2"></view>

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
        <view class="course-card" v-for="item in courses" :key="item.id" hover-class="card-hover">
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
        <view class="plan-card" v-for="item in plans" :key="item.id" hover-class="card-hover">
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
        <view class="resource-card" v-for="item in resources" :key="item.id" hover-class="card-hover">
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
import { ref } from 'vue'

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

const courses = ref<Course[]>([
  { id: 1, name: 'Vue3从入门到实战', teacher: '张老师', learners: 2386, progress: 68, bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)' },
  { id: 2, name: 'TypeScript高级编程', teacher: '李老师', learners: 1562, progress: 35, bg: 'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)' },
  { id: 3, name: 'React Hooks深度解析', teacher: '王老师', learners: 1890, progress: 0, bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)' },
  { id: 4, name: 'Node.js后端开发', teacher: '赵老师', learners: 1245, progress: 92, bg: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)' },
  { id: 5, name: 'CSS动画与交互设计', teacher: '刘老师', learners: 978, progress: 15, bg: 'linear-gradient(135deg, #fa709a 0%, #fee140 100%)' },
  { id: 6, name: '微信小程序开发实战', teacher: '陈老师', learners: 3120, progress: 50, bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)' }
])

const plans = ref<Plan[]>([
  { id: 1, name: '前端工程师30天进阶', progress: 68, days: 20 },
  { id: 2, name: '每日算法打卡', progress: 45, days: 15 },
  { id: 3, name: '英语口语提升计划', progress: 30, days: 9 },
  { id: 4, name: 'Python数据分析入门', progress: 80, days: 24 },
  { id: 5, name: '产品设计思维训练', progress: 20, days: 6 }
])

const resources = ref<Resource[]>([
  { id: 1, name: 'Vue3官方文档精编', type: 'PDF', typeIcon: 'PDF', size: '12.5MB', bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)', downloaded: false },
  { id: 2, name: 'TypeScript速查手册', type: 'PDF', typeIcon: 'PDF', size: '3.2MB', bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)', downloaded: true },
  { id: 3, name: '前端面试题集2026', type: 'DOC', typeIcon: 'DOC', size: '8.7MB', bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)', downloaded: false },
  { id: 4, name: 'CSS布局实战视频', type: '视频', typeIcon: 'MP4', size: '256MB', bg: 'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)', downloaded: false },
  { id: 5, name: 'React源码解析', type: 'PDF', typeIcon: 'PDF', size: '15.3MB', bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)', downloaded: false },
  { id: 6, name: 'Node.js最佳实践', type: 'EPUB', typeIcon: 'EPUB', size: '6.8MB', bg: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)', downloaded: true }
])

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
  left: -80rpx;
}

.orb-2 {
  width: 350rpx;
  height: 350rpx;
  background: #f093fb;
  opacity: 0.08;
  top: 280rpx;
  right: -100rpx;
}

.stats-bar {
  display: flex;
  padding: 24rpx;
  gap: 16rpx;
  position: relative;
  z-index: 1;
}

.stat-card {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 24rpx 0;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  border-radius: 24rpx;
  backdrop-filter: blur(20px);
  transition: all 0.3s;
}

.card-hover {
  transform: scale(0.95);
  opacity: 0.85;
}

.stat-value {
  font-size: 36rpx;
  font-weight: 700;
  background: linear-gradient(135deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.stat-label {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.6);
  margin-top: 8rpx;
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

.course-card {
  display: flex;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  border-radius: 24rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  backdrop-filter: blur(20px);
  transition: all 0.3s;
}

.course-cover {
  width: 180rpx;
  height: 180rpx;
  border-radius: 16rpx;
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
  font-weight: 700;
  color: #FFFFFF;
}

.course-teacher {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.6);
  margin-top: 8rpx;
}

.course-people {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.4);
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
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4rpx;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #667eea, #764ba2);
  border-radius: 4rpx;
}

.progress-text {
  font-size: 20rpx;
  color: rgba(255, 255, 255, 0.4);
  margin-left: 12rpx;
}

.plan-card {
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  border-radius: 24rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  backdrop-filter: blur(20px);
  transition: all 0.3s;
}

.plan-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.plan-name {
  font-size: 30rpx;
  font-weight: 700;
  color: #FFFFFF;
}

.plan-days {
  font-size: 24rpx;
  color: #FFFFFF;
  background: linear-gradient(135deg, #667eea, #764ba2);
  padding: 6rpx 16rpx;
  border-radius: 16rpx;
}

.plan-progress-wrap {
  display: flex;
  align-items: center;
  margin-top: 20rpx;
}

.plan-btn {
  margin-top: 20rpx;
  padding: 14rpx 0;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-radius: 40rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4rpx 20rpx rgba(102, 126, 234, 0.4);
}

.plan-btn-text {
  font-size: 26rpx;
  color: #FFFFFF;
}

.resource-card {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  border-radius: 24rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  backdrop-filter: blur(20px);
  transition: all 0.3s;
}

.resource-icon {
  width: 72rpx;
  height: 72rpx;
  border-radius: 16rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.resource-icon-text {
  font-size: 22rpx;
  font-weight: 700;
  color: #FFFFFF;
}

.resource-info {
  flex: 1;
  margin-left: 20rpx;
}

.resource-name {
  font-size: 28rpx;
  font-weight: 600;
  color: #FFFFFF;
}

.resource-meta {
  display: flex;
  align-items: center;
  gap: 16rpx;
  margin-top: 8rpx;
}

.resource-type {
  font-size: 22rpx;
  background: linear-gradient(135deg, #4facfe, #00f2fe);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.resource-size {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.4);
}

.download-btn {
  padding: 10rpx 24rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-radius: 40rpx;
  box-shadow: 0 4rpx 20rpx rgba(102, 126, 234, 0.4);
  flex-shrink: 0;
}

.download-btn.downloaded {
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  box-shadow: none;
}

.download-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
}

.download-btn-text.downloaded {
  color: rgba(255, 255, 255, 0.4);
}
</style>
