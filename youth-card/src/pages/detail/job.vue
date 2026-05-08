<template>
  <view class="page">
    <view class="orb orb-1"></view>
    <view class="orb orb-2"></view>

    <view class="search-bar">
      <view class="search-input-wrap">
        <text class="search-icon">🔍</text>
        <input class="search-input" placeholder="搜索职位" placeholder-class="search-placeholder" v-model="keyword" />
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
      <view class="job-card" v-for="item in filteredJobs" :key="item.id" hover-class="card-hover">
        <view class="job-header">
          <text class="job-name">{{ item.name }}</text>
          <text class="job-salary">{{ item.salary }}</text>
        </view>
        <text class="job-company">{{ item.company }}</text>
        <view class="job-tags">
          <text class="job-tag" v-for="tag in item.tags" :key="tag">{{ tag }}</text>
        </view>
        <view class="job-bottom">
          <text class="job-meta">{{ item.location }} · {{ item.time }}</text>
          <view class="apply-btn" :class="{ applied: item.applied }" @click="onApply(item)">
            <text class="apply-btn-text" :class="{ applied: item.applied }">{{ item.applied ? '已投递' : '投递' }}</text>
          </view>
        </view>
      </view>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Job {
  id: number
  name: string
  company: string
  salary: string
  location: string
  tags: string[]
  time: string
  category: string
  applied: boolean
}

const keyword = ref('')
const currentCategory = ref('推荐')
const categories = ['推荐', '技术', '设计', '运营', '市场', '产品']

const jobs = ref<Job[]>([
  { id: 1, name: '前端开发工程师', company: '星辰科技有限公司', salary: '8K-15K', location: '城东', tags: ['五险一金', '弹性工作'], time: '2天前', category: '技术', applied: false },
  { id: 2, name: 'Java后端开发', company: '云端网络科技', salary: '10K-20K', location: '城西', tags: ['五险一金', '年终奖', '远程'], time: '1天前', category: '技术', applied: false },
  { id: 3, name: 'UI设计师', company: '创想设计工作室', salary: '7K-12K', location: '城南', tags: ['弹性工作', '远程'], time: '3天前', category: '设计', applied: false },
  { id: 4, name: '视觉设计师', company: '美图传媒', salary: '8K-14K', location: '城东', tags: ['五险一金', '餐补'], time: '1天前', category: '设计', applied: true },
  { id: 5, name: '新媒体运营', company: '潮流文化', salary: '6K-10K', location: '城北', tags: ['弹性工作', '下午茶'], time: '5天前', category: '运营', applied: false },
  { id: 6, name: '用户运营专员', company: '乐享科技', salary: '7K-11K', location: '城东', tags: ['五险一金', '带薪年假'], time: '2天前', category: '运营', applied: false },
  { id: 7, name: '市场推广经理', company: '锐步商贸', salary: '8K-16K', location: '城西', tags: ['五险一金', '绩效奖金'], time: '4天前', category: '市场', applied: false },
  { id: 8, name: '产品经理', company: '智联科技', salary: '12K-25K', location: '城南', tags: ['五险一金', '弹性工作', '远程'], time: '1天前', category: '产品', applied: false }
])

const filteredJobs = computed(() => {
  return jobs.value.filter(item => {
    const matchCategory = currentCategory.value === '推荐' || item.category === currentCategory.value
    const matchKeyword = !keyword.value || item.name.includes(keyword.value) || item.company.includes(keyword.value)
    return matchCategory && matchKeyword
  })
})

const onApply = (item: Job) => {
  if (item.applied) return
  item.applied = true
  uni.showToast({ title: '投递成功', icon: 'success' })
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
  top: -80rpx;
  left: -120rpx;
}

.orb-2 {
  width: 300rpx;
  height: 300rpx;
  background: #f5576c;
  opacity: 0.1;
  top: 280rpx;
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

.job-card {
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  border-radius: 24rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  backdrop-filter: blur(20px);
  transition: all 0.3s;
}

.card-hover {
  transform: scale(0.95);
  opacity: 0.85;
}

.job-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.job-name {
  font-size: 32rpx;
  font-weight: 700;
  color: #FFFFFF;
}

.job-salary {
  font-size: 30rpx;
  font-weight: 700;
  background: linear-gradient(135deg, #f093fb, #f5576c);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.job-company {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.6);
  margin-top: 12rpx;
}

.job-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 12rpx;
  margin-top: 16rpx;
}

.job-tag {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.6);
  background: transparent;
  border: 1rpx solid rgba(102, 126, 234, 0.4);
  padding: 6rpx 16rpx;
  border-radius: 16rpx;
}

.job-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 20rpx;
}

.job-meta {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.4);
}

.apply-btn {
  padding: 10rpx 36rpx;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-radius: 40rpx;
  box-shadow: 0 4rpx 20rpx rgba(102, 126, 234, 0.4);
}

.apply-btn.applied {
  background: rgba(255, 255, 255, 0.06);
  border: 1rpx solid rgba(255, 255, 255, 0.1);
  box-shadow: none;
}

.apply-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
}

.apply-btn-text.applied {
  color: rgba(255, 255, 255, 0.4);
}
</style>
