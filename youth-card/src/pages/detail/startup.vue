<template>
  <view class="page">
    <view class="stats-bar">
      <view class="stat-card">
        <text class="stat-value">128</text>
        <text class="stat-label">创业项目</text>
      </view>
      <view class="stat-card">
        <text class="stat-value">36</text>
        <text class="stat-label">孵化器</text>
      </view>
      <view class="stat-card">
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
        <view class="project-card" v-for="item in projects" :key="item.id">
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
        <view class="incubator-card" v-for="item in incubators" :key="item.id">
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
          <view class="incubator-btn" @click="onApplyIncubator(item)">
            <text class="incubator-btn-text">{{ item.applied ? '已申请' : '申请入驻' }}</text>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '政策扶持'">
        <view class="policy-card" v-for="item in policies" :key="item.id">
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
        <view class="finance-card" v-for="item in finances" :key="item.id">
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
import { ref } from 'vue'

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

const onApplyIncubator = (item: Incubator) => {
  if (item.applied) return
  item.applied = true
  uni.showToast({ title: '申请已提交', icon: 'success' })
}
</script>

<style>
.page {
  min-height: 100vh;
  background-color: #F5F6FA;
  display: flex;
  flex-direction: column;
}

.stats-bar {
  display: flex;
  padding: 24rpx;
  gap: 16rpx;
  background-color: #FFFFFF;
}

.stat-card {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 24rpx 0;
  background: linear-gradient(135deg, #4F6EF7 0%, #7B9AFF 100%);
  border-radius: 20rpx;
}

.stat-value {
  font-size: 36rpx;
  font-weight: 700;
  color: #FFFFFF;
}

.stat-label {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.85);
  margin-top: 8rpx;
}

.category-scroll {
  white-space: nowrap;
  background-color: #FFFFFF;
  padding: 16rpx 24rpx;
  border-bottom: 1rpx solid #F0F0F0;
}

.category-tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12rpx 32rpx;
  border-radius: 32rpx;
  background-color: #F0F2F5;
  margin-right: 16rpx;
}

.category-tag.active {
  background-color: #4F6EF7;
}

.category-text {
  font-size: 26rpx;
  color: #666666;
}

.category-text.active-text {
  color: #FFFFFF;
  font-weight: 600;
}

.list-scroll {
  flex: 1;
  padding: 20rpx 24rpx;
}

.project-card {
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.project-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.project-name {
  font-size: 32rpx;
  font-weight: 700;
  color: #333333;
}

.project-stage {
  font-size: 22rpx;
  color: #4F6EF7;
  background-color: rgba(79, 110, 247, 0.1);
  padding: 6rpx 16rpx;
  border-radius: 16rpx;
}

.project-desc {
  font-size: 26rpx;
  color: #666666;
  margin-top: 12rpx;
  line-height: 1.6;
}

.project-bottom {
  margin-top: 16rpx;
}

.project-funding {
  font-size: 24rpx;
  color: #999999;
  background-color: #F5F6FA;
  padding: 6rpx 16rpx;
  border-radius: 16rpx;
}

.project-funding.funded {
  color: #FF6B6B;
  background-color: rgba(255, 107, 107, 0.1);
}

.incubator-card {
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.incubator-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.incubator-name {
  font-size: 32rpx;
  font-weight: 700;
  color: #333333;
}

.incubator-address {
  font-size: 24rpx;
  color: #999999;
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
  color: #4F6EF7;
}

.incubator-stat-label {
  font-size: 22rpx;
  color: #999999;
  margin-top: 4rpx;
}

.incubator-btn {
  margin-top: 20rpx;
  padding: 14rpx 0;
  background-color: #4F6EF7;
  border-radius: 28rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.incubator-btn-text {
  font-size: 26rpx;
  color: #FFFFFF;
}

.policy-card {
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.policy-name {
  font-size: 30rpx;
  font-weight: 700;
  color: #333333;
}

.policy-meta {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 16rpx;
}

.policy-dept {
  font-size: 24rpx;
  color: #4F6EF7;
}

.policy-period {
  font-size: 22rpx;
  color: #999999;
}

.finance-card {
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.finance-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.finance-name {
  font-size: 32rpx;
  font-weight: 700;
  color: #333333;
}

.finance-stage {
  font-size: 22rpx;
  color: #4F6EF7;
  background-color: rgba(79, 110, 247, 0.1);
  padding: 6rpx 16rpx;
  border-radius: 16rpx;
}

.finance-amount {
  font-size: 28rpx;
  font-weight: 600;
  color: #FF6B6B;
  margin-top: 16rpx;
}
</style>
