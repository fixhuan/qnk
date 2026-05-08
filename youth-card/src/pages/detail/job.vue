<template>
  <view class="page">
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
          <view class="job-actions">
            <view class="fav-btn" @click="onToggleFav(item)">
              <text class="fav-icon" :class="{ favorited: item.favorited }">{{ item.favorited ? '❤' : '♡' }}</text>
            </view>
            <view class="apply-btn" :class="{ applied: item.applied }" @click="onApply(item)">
              <text class="apply-btn-text" :class="{ applied: item.applied }">{{ item.applied ? '已投递' : '投递' }}</text>
            </view>
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
  favorited: boolean
}

const keyword = ref('')
const currentCategory = ref('推荐')
const categories = ['推荐', '技术', '设计', '运营', '市场', '产品']

const jobs = ref<Job[]>([
  { id: 1, name: '前端开发工程师', company: '星辰科技有限公司', salary: '8K-15K', location: '城东', tags: ['五险一金', '弹性工作'], time: '2天前', category: '技术', applied: false, favorited: false },
  { id: 2, name: 'Java后端开发', company: '云端网络科技', salary: '10K-20K', location: '城西', tags: ['五险一金', '年终奖', '远程'], time: '1天前', category: '技术', applied: false, favorited: false },
  { id: 3, name: 'UI设计师', company: '创想设计工作室', salary: '7K-12K', location: '城南', tags: ['弹性工作', '远程'], time: '3天前', category: '设计', applied: false, favorited: true },
  { id: 4, name: '视觉设计师', company: '美图传媒', salary: '8K-14K', location: '城东', tags: ['五险一金', '餐补'], time: '1天前', category: '设计', applied: true, favorited: false },
  { id: 5, name: '新媒体运营', company: '潮流文化', salary: '6K-10K', location: '城北', tags: ['弹性工作', '下午茶'], time: '5天前', category: '运营', applied: false, favorited: false },
  { id: 6, name: '用户运营专员', company: '乐享科技', salary: '7K-11K', location: '城东', tags: ['五险一金', '带薪年假'], time: '2天前', category: '运营', applied: false, favorited: false },
  { id: 7, name: '市场推广经理', company: '锐步商贸', salary: '8K-16K', location: '城西', tags: ['五险一金', '绩效奖金'], time: '4天前', category: '市场', applied: false, favorited: false },
  { id: 8, name: '产品经理', company: '智联科技', salary: '12K-25K', location: '城南', tags: ['五险一金', '弹性工作', '远程'], time: '1天前', category: '产品', applied: false, favorited: false }
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

const onToggleFav = (item: Job) => {
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

.job-card {
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.card-hover {
  opacity: 0.6;
}

.job-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.job-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1a1612;
}

.job-salary {
  font-size: 30rpx;
  font-weight: 600;
  color: #c2410c;
}

.job-company {
  font-size: 26rpx;
  color: #6b5e52;
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
  color: #a16207;
  background: #fefce8;
  padding: 6rpx 16rpx;
  border-radius: 999rpx;
}

.job-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 20rpx;
}

.job-actions {
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

.job-meta {
  font-size: 22rpx;
  color: #a89888;
}

.apply-btn {
  padding: 10rpx 36rpx;
  background: #c2410c;
  border-radius: 8rpx;
}

.apply-btn.applied {
  background: #f5f0ea;
}

.apply-btn-text {
  font-size: 24rpx;
  color: #ffffff;
}

.apply-btn-text.applied {
  color: #a89888;
}
</style>
