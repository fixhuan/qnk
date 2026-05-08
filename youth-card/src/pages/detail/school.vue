<template>
  <view class="page">
    <view class="search-bar">
      <view class="search-input-wrap">
        <text class="search-icon">🔍</text>
        <input class="search-input" placeholder="搜索课程" placeholder-class="search-placeholder" v-model="keyword" />
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
      <view class="course-card" v-for="item in filteredCourses" :key="item.id" hover-class="card-hover">
        <view class="course-cover" :style="{ background: item.bg }"></view>
        <view class="course-info">
          <text class="course-name">{{ item.name }}</text>
          <text class="course-teacher">{{ item.teacher }}</text>
          <text class="course-time">{{ item.time }}</text>
          <view class="course-progress-wrap">
            <view class="progress-bar">
              <view class="progress-fill" :style="{ width: (item.enrolledCount / item.total * 100) + '%' }"></view>
            </view>
            <text class="progress-text">{{ item.enrolledCount }}/{{ item.total }}</text>
          </view>
          <view class="course-bottom">
            <text class="course-price" :class="{ free: item.price === 0 }">{{ item.price === 0 ? '免费' : '¥' + item.price }}</text>
            <view class="course-actions">
              <view class="fav-btn" @click="onToggleFav(item)">
                <text class="fav-icon" :class="{ favorited: item.favorited }">{{ item.favorited ? '❤' : '♡' }}</text>
              </view>
              <view class="enroll-btn" :class="{ enrolled: item.enrolled }" @click="onEnroll(item)">
                <text class="enroll-btn-text" :class="{ enrolled: item.enrolled }">{{ item.enrolled ? '已报名' : '报名' }}</text>
              </view>
            </view>
          </view>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Course {
  id: number
  name: string
  teacher: string
  time: string
  enrolled: boolean
  enrolledCount: number
  total: number
  price: number
  category: string
  bg: string
  favorited: boolean
}

const keyword = ref('')
const currentCategory = ref('全部')
const categories = ['全部', '技能培训', '语言学习', '艺术修养', '职业资格']

const courses = ref<Course[]>([
  { id: 1, name: 'Python编程入门', teacher: '张明老师', time: '每周二、四 19:00-21:00', enrolled: false, enrolledCount: 28, total: 40, price: 0, category: '技能培训', bg: '#fef3ee', favorited: false },
  { id: 2, name: '短视频剪辑实战', teacher: '李华老师', time: '每周一、三 19:30-21:00', enrolled: false, enrolledCount: 35, total: 40, price: 299, category: '技能培训', bg: '#eff6ff', favorited: false },
  { id: 3, name: '日语N3精讲班', teacher: '田中老师', time: '每周三、五 18:30-20:30', enrolled: true, enrolledCount: 30, total: 35, price: 599, category: '语言学习', bg: '#fefce8', favorited: true },
  { id: 4, name: '商务英语口语', teacher: 'Sarah老师', time: '每周二、四 20:00-21:30', enrolled: false, enrolledCount: 18, total: 30, price: 499, category: '语言学习', bg: '#f0fdf4', favorited: false },
  { id: 5, name: '水彩画基础', teacher: '王艺老师', time: '每周六 14:00-17:00', enrolled: false, enrolledCount: 15, total: 25, price: 0, category: '艺术修养', bg: '#fef3ee', favorited: false },
  { id: 6, name: '吉他弹唱入门', teacher: '刘乐老师', time: '每周日 10:00-12:00', enrolled: false, enrolledCount: 22, total: 30, price: 399, category: '艺术修养', bg: '#fefce8', favorited: false },
  { id: 7, name: '人力资源管理师', teacher: '陈老师', time: '每周六 9:00-12:00', enrolled: false, enrolledCount: 32, total: 50, price: 899, category: '职业资格', bg: '#eff6ff', favorited: false },
  { id: 8, name: '心理咨询师培训', teacher: '赵老师', time: '每周日 14:00-17:00', enrolled: false, enrolledCount: 20, total: 40, price: 1299, category: '职业资格', bg: '#f0fdf4', favorited: false }
])

const filteredCourses = computed(() => {
  return courses.value.filter(item => {
    const matchCategory = currentCategory.value === '全部' || item.category === currentCategory.value
    const matchKeyword = !keyword.value || item.name.includes(keyword.value) || item.teacher.includes(keyword.value)
    return matchCategory && matchKeyword
  })
})

const onEnroll = (item: Course) => {
  if (item.enrolled) return
  item.enrolled = true
  item.enrolledCount++
  uni.showToast({ title: '报名成功', icon: 'success' })
}

const onToggleFav = (item: Course) => {
  item.favorited = !item.favorited
  uni.showToast({ title: item.favorited ? '已收藏' : '已取消收藏', icon: 'none' })
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

.card-hover {
  opacity: 0.6;
}

.course-cover {
  width: 180rpx;
  height: 200rpx;
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

.course-time {
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

.course-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 12rpx;
}

.course-actions {
  display: flex;
  align-items: center;
  gap: 16rpx;
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

.course-price {
  font-size: 32rpx;
  font-weight: 600;
  color: #c2410c;
}

.course-price.free {
  color: #15803d;
  background: #f0fdf4;
  font-size: 24rpx;
  padding: 4rpx 16rpx;
  border-radius: 999rpx;
}

.enroll-btn {
  padding: 10rpx 32rpx;
  background: #c2410c;
  border-radius: 8rpx;
}

.enroll-btn.enrolled {
  background: #f5f0ea;
}

.enroll-btn-text {
  font-size: 24rpx;
  color: #ffffff;
}

.enroll-btn-text.enrolled {
  color: #a89888;
}
</style>
