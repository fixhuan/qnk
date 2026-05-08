<template>
  <view class="page">
    <view class="orb orb-1"></view>
    <view class="orb orb-2"></view>

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
            <view class="enroll-btn" :class="{ enrolled: item.enrolled }" @click="onEnroll(item)">
              <text class="enroll-btn-text" :class="{ enrolled: item.enrolled }">{{ item.enrolled ? '已报名' : '报名' }}</text>
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
}

const keyword = ref('')
const currentCategory = ref('全部')
const categories = ['全部', '技能培训', '语言学习', '艺术修养', '职业资格']

const courses = ref<Course[]>([
  { id: 1, name: 'Python编程入门', teacher: '张明老师', time: '每周二、四 19:00-21:00', enrolled: false, enrolledCount: 28, total: 40, price: 0, category: '技能培训', bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)' },
  { id: 2, name: '短视频剪辑实战', teacher: '李华老师', time: '每周一、三 19:30-21:00', enrolled: false, enrolledCount: 35, total: 40, price: 299, category: '技能培训', bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)' },
  { id: 3, name: '日语N3精讲班', teacher: '田中老师', time: '每周三、五 18:30-20:30', enrolled: true, enrolledCount: 30, total: 35, price: 599, category: '语言学习', bg: 'linear-gradient(135deg, #fa709a 0%, #fee140 100%)' },
  { id: 4, name: '商务英语口语', teacher: 'Sarah老师', time: '每周二、四 20:00-21:30', enrolled: false, enrolledCount: 18, total: 30, price: 499, category: '语言学习', bg: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)' },
  { id: 5, name: '水彩画基础', teacher: '王艺老师', time: '每周六 14:00-17:00', enrolled: false, enrolledCount: 15, total: 25, price: 0, category: '艺术修养', bg: 'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)' },
  { id: 6, name: '吉他弹唱入门', teacher: '刘乐老师', time: '每周日 10:00-12:00', enrolled: false, enrolledCount: 22, total: 30, price: 399, category: '艺术修养', bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)' },
  { id: 7, name: '人力资源管理师', teacher: '陈老师', time: '每周六 9:00-12:00', enrolled: false, enrolledCount: 32, total: 50, price: 899, category: '职业资格', bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)' },
  { id: 8, name: '心理咨询师培训', teacher: '赵老师', time: '每周日 14:00-17:00', enrolled: false, enrolledCount: 20, total: 40, price: 1299, category: '职业资格', bg: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)' }
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
  background: #764ba2;
  opacity: 0.15;
  top: -100rpx;
  left: -100rpx;
}

.orb-2 {
  width: 300rpx;
  height: 300rpx;
  background: #4facfe;
  opacity: 0.1;
  top: 300rpx;
  right: -80rpx;
}

.search-bar {
  padding: 20rpx 24rpx;
  position: relative;
  z-index: 1;
}

.search-input-wrap {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  border-radius: 40rpx;
  padding: 16rpx 24rpx;
  backdrop-filter: blur(20px);
}

.search-icon {
  font-size: 28rpx;
  margin-right: 12rpx;
}

.search-input {
  flex: 1;
  font-size: 28rpx;
  color: #FFFFFF;
}

.search-placeholder {
  color: rgba(255, 255, 255, 0.4);
  font-size: 28rpx;
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

.card-hover {
  transform: scale(0.95);
  opacity: 0.85;
}

.course-cover {
  width: 180rpx;
  height: 200rpx;
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

.course-time {
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

.course-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 12rpx;
}

.course-price {
  font-size: 32rpx;
  font-weight: 700;
  background: linear-gradient(135deg, #f093fb, #f5576c);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.course-price.free {
  background: linear-gradient(135deg, #43e97b, #38f9d7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.enroll-btn {
  padding: 10rpx 32rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-radius: 40rpx;
  box-shadow: 0 4rpx 20rpx rgba(102, 126, 234, 0.4);
}

.enroll-btn.enrolled {
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  box-shadow: none;
}

.enroll-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
}

.enroll-btn-text.enrolled {
  color: rgba(255, 255, 255, 0.4);
}
</style>
