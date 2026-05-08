<template>
  <view class="page">
    <view class="search-bar">
      <view class="search-input-wrap">
        <text class="search-icon">🔍</text>
        <input class="search-input" placeholder="搜索创业项目、孵化器、政策" placeholder-class="search-placeholder" v-model="keyword" />
      </view>
    </view>

    <view class="stats-bar">
      <view class="stat-card" hover-class="card-hover">
        <text class="stat-value">128</text>
        <text class="stat-label">创业项目</text>
      </view>
      <view class="stat-card" hover-class="card-hover">
        <text class="stat-value">36</text>
        <text class="stat-label">孵化器</text>
      </view>
      <view class="stat-card" hover-class="card-hover">
        <text class="stat-value">2.8亿</text>
        <text class="stat-label">融资金额</text>
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
      <template v-if="currentCategory === '创业项目'">
        <view class="project-card" v-for="item in filteredProjects" :key="item.id" hover-class="card-hover" @click="onProjectClick(item)">
          <view class="project-header">
            <text class="project-name">{{ item.name }}</text>
            <text class="project-stage">{{ item.stage }}</text>
          </view>
          <text class="project-desc">{{ item.desc }}</text>
          <view class="project-bottom">
            <text class="project-funding" :class="{ funded: item.funding !== '未融资' }">{{ item.funding }}</text>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '孵化器'">
        <view class="incubator-card" v-for="item in filteredIncubators" :key="item.id" hover-class="card-hover">
          <view class="incubator-header">
            <text class="incubator-name">{{ item.name }}</text>
          </view>
          <text class="incubator-address">{{ item.address }}</text>
          <view class="incubator-stats">
            <view class="incubator-stat">
              <text class="incubator-stat-value">{{ item.companies }}</text>
              <text class="incubator-stat-label">入驻企业</text>
            </view>
            <view class="incubator-stat">
              <text class="incubator-stat-value">{{ item.vacancy }}</text>
              <text class="incubator-stat-label">空位</text>
            </view>
          </view>
          <view class="incubator-btn" :class="{ applied: item.applied }" @click="onApplyIncubator(item)">
            <text class="incubator-btn-text" :class="{ applied: item.applied }">{{ item.applied ? '已申请' : '申请入驻' }}</text>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '政策扶持'">
        <view class="policy-card" v-for="item in filteredPolicies" :key="item.id" hover-class="card-hover" @click="onPolicyClick(item)">
          <view class="policy-header">
            <text class="policy-name">{{ item.name }}</text>
          </view>
          <view class="policy-meta">
            <text class="policy-dept">{{ item.dept }}</text>
            <text class="policy-period">有效期至 {{ item.period }}</text>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '融资对接'">
        <view class="finance-card" v-for="item in filteredFinances" :key="item.id" hover-class="card-hover" @click="onFinanceClick(item)">
          <view class="finance-header">
            <text class="finance-name">{{ item.name }}</text>
            <text class="finance-stage">{{ item.stage }}</text>
          </view>
          <text class="finance-amount">融资金额：{{ item.amount }}</text>
        </view>
      </template>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Project {
  id: number
  name: string
  desc: string
  stage: string
  funding: string
}

interface Incubator {
  id: number
  name: string
  address: string
  companies: number
  vacancy: number
  applied: boolean
}

interface Policy {
  id: number
  name: string
  dept: string
  period: string
}

interface Finance {
  id: number
  name: string
  amount: string
  stage: string
}

const currentCategory = ref('创业项目')
const categories = ['创业项目', '孵化器', '政策扶持', '融资对接']
const keyword = ref('')

const projects = ref<Project[]>([
  { id: 1, name: '智慧校园', desc: '基于AI的校园智能管理系统，提升校园管理效率', stage: '种子期', funding: '天使轮' },
  { id: 2, name: '绿行出行', desc: '新能源共享出行平台，倡导绿色出行方式', stage: '成长期', funding: 'A轮' },
  { id: 3, name: '味享科技', desc: '餐饮供应链数字化解决方案', stage: '初创期', funding: '未融资' },
  { id: 4, name: '医养云', desc: '社区医养结合服务平台', stage: '成长期', funding: 'B轮' },
  { id: 5, name: '创学空间', desc: '在线职业技能培训平台', stage: '种子期', funding: '天使轮' },
  { id: 6, name: '宠趣生活', desc: '宠物健康管理与社交平台', stage: '初创期', funding: '种子轮' }
])

const incubators = ref<Incubator[]>([
  { id: 1, name: '青年创业孵化基地', address: '城东区创新路88号', companies: 45, vacancy: 12, applied: false },
  { id: 2, name: '科技企业孵化中心', address: '城西区科技大道126号', companies: 68, vacancy: 8, applied: false },
  { id: 3, name: '互联网+产业园', address: '城南新区创业街56号', companies: 92, vacancy: 15, applied: true },
  { id: 4, name: '文化创意孵化器', address: '城北文化路32号', companies: 38, vacancy: 20, applied: false },
  { id: 5, name: '数字经济产业园', address: '城东区数字大道99号', companies: 56, vacancy: 10, applied: false }
])

const policies = ref<Policy[]>([
  { id: 1, name: '青年创业补贴政策', dept: '市人社局', period: '2026-12-31' },
  { id: 2, name: '小微企业税收优惠', dept: '市税务局', period: '2027-06-30' },
  { id: 3, name: '创业担保贷款政策', dept: '市金融局', period: '2026-12-31' },
  { id: 4, name: '人才引进补贴', dept: '市委人才办', period: '2027-03-31' },
  { id: 5, name: '科技创新奖励办法', dept: '市科技局', period: '2026-09-30' },
  { id: 6, name: '大学生创业扶持计划', dept: '市教育局', period: '2027-01-31' }
])

const finances = ref<Finance[]>([
  { id: 1, name: '智慧校园', amount: '500万', stage: '天使轮' },
  { id: 2, name: '绿行出行', amount: '2000万', stage: 'A轮' },
  { id: 3, name: '医养云', amount: '5000万', stage: 'B轮' },
  { id: 4, name: '创学空间', amount: '300万', stage: '天使轮' },
  { id: 5, name: '宠趣生活', amount: '800万', stage: '种子轮' }
])

const filteredProjects = computed(() => {
  return projects.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value) || item.desc.includes(keyword.value)
  })
})

const filteredIncubators = computed(() => {
  return incubators.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value) || item.address.includes(keyword.value)
  })
})

const filteredPolicies = computed(() => {
  return policies.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value) || item.dept.includes(keyword.value)
  })
})

const filteredFinances = computed(() => {
  return finances.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value) || item.amount.includes(keyword.value)
  })
})

const onProjectClick = (item: Project) => {
  uni.showToast({ title: item.name, icon: 'none' })
}

const onPolicyClick = (item: Policy) => {
  uni.showToast({ title: item.name, icon: 'none' })
}

const onFinanceClick = (item: Finance) => {
  uni.showToast({ title: `${item.name} · ${item.amount}`, icon: 'none' })
}

const onApplyIncubator = (item: Incubator) => {
  if (item.applied) return
  item.applied = true
  uni.showToast({ title: '申请已提交', icon: 'success' })
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

.project-card {
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.project-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.project-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1a1612;
}

.project-stage {
  font-size: 22rpx;
  color: #c2410c;
  background: #fef3ee;
  padding: 6rpx 16rpx;
  border-radius: 999rpx;
}

.project-desc {
  font-size: 26rpx;
  color: #6b5e52;
  margin-top: 12rpx;
  line-height: 1.6;
}

.project-bottom {
  margin-top: 16rpx;
}

.project-funding {
  font-size: 24rpx;
  color: #a89888;
  background: #f5f0ea;
  padding: 6rpx 16rpx;
  border-radius: 999rpx;
}

.project-funding.funded {
  color: #c2410c;
  background: #fef3ee;
}

.incubator-card {
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.incubator-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.incubator-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1a1612;
}

.incubator-address {
  font-size: 24rpx;
  color: #a89888;
  margin-top: 8rpx;
}

.incubator-stats {
  display: flex;
  gap: 40rpx;
  margin-top: 20rpx;
}

.incubator-stat {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.incubator-stat-value {
  font-size: 32rpx;
  font-weight: 700;
  color: #c2410c;
}

.incubator-stat-label {
  font-size: 22rpx;
  color: #a89888;
  margin-top: 4rpx;
}

.incubator-btn {
  margin-top: 20rpx;
  padding: 14rpx 0;
  background: #c2410c;
  border-radius: 8rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.incubator-btn.applied {
  background: #f5f0ea;
}

.incubator-btn-text {
  font-size: 26rpx;
  color: #ffffff;
}

.incubator-btn-text.applied {
  color: #a89888;
}

.policy-card {
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.policy-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #1a1612;
}

.policy-meta {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 16rpx;
}

.policy-dept {
  font-size: 24rpx;
  color: #1d4ed8;
  background: #eff6ff;
  padding: 4rpx 12rpx;
  border-radius: 999rpx;
}

.policy-period {
  font-size: 22rpx;
  color: #a89888;
}

.finance-card {
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
}

.finance-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.finance-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1a1612;
}

.finance-stage {
  font-size: 22rpx;
  color: #c2410c;
  background: #fef3ee;
  padding: 6rpx 16rpx;
  border-radius: 999rpx;
}

.finance-amount {
  font-size: 28rpx;
  font-weight: 600;
  color: #c2410c;
  margin-top: 16rpx;
}
</style>
