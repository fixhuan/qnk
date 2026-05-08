<template>
  <view class="page">
    <view class="stats-bar">
      <view class="stat-card">
        <text class="stat-value">126</text>
        <text class="stat-label">累计志愿时长</text>
      </view>
      <view class="stat-card">
        <text class="stat-value">28</text>
        <text class="stat-label">参与活动</text>
      </view>
      <view class="stat-card">
        <text class="stat-value">5</text>
        <text class="stat-label">获得证书</text>
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
      <template v-if="currentCategory === '志愿活动'">
        <view class="volunteer-card" v-for="item in volunteerActivities" :key="item.id">
          <view class="volunteer-header">
            <text class="volunteer-name">{{ item.name }}</text>
          </view>
          <text class="volunteer-time">🕐 {{ item.time }}</text>
          <text class="volunteer-location">📍 {{ item.location }}</text>
          <view class="volunteer-meta">
            <text class="volunteer-hours">服务时长：{{ item.hours }}小时</text>
            <text class="volunteer-people">{{ item.people }}人已报名</text>
          </view>
          <view class="volunteer-btn" :class="{ joined: item.joined }" @click="onJoinVolunteer(item)">
            <text class="volunteer-btn-text" :class="{ joined: item.joined }">{{ item.joined ? '已报名' : '报名参加' }}</text>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '组织机构'">
        <view class="org-card" v-for="item in organizations" :key="item.id">
          <view class="org-header">
            <view class="org-avatar" :style="{ background: item.bg }"></view>
            <view class="org-info">
              <text class="org-name">{{ item.name }}</text>
              <text class="org-desc">{{ item.desc }}</text>
            </view>
          </view>
          <view class="org-bottom">
            <text class="org-activities">开展活动 {{ item.activities }}次</text>
            <view class="follow-btn" :class="{ followed: item.followed }" @click="onFollow(item)">
              <text class="follow-btn-text" :class="{ followed: item.followed }">{{ item.followed ? '已关注' : '关注' }}</text>
            </view>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '志愿证书'">
        <view class="cert-card" v-for="item in certificates" :key="item.id">
          <view class="cert-icon">🏅</view>
          <view class="cert-info">
            <text class="cert-name">{{ item.name }}</text>
            <text class="cert-org">颁发机构：{{ item.org }}</text>
            <text class="cert-time">获得时间：{{ item.time }}</text>
            <text class="cert-no">编号：{{ item.no }}</text>
          </view>
        </view>
      </template>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface VolunteerActivity {
  id: number
  name: string
  time: string
  location: string
  hours: number
  people: number
  joined: boolean
}

interface Organization {
  id: number
  name: string
  desc: string
  activities: number
  bg: string
  followed: boolean
}

interface Certificate {
  id: number
  name: string
  org: string
  time: string
  no: string
}

const currentCategory = ref('志愿活动')
const categories = ['志愿活动', '组织机构', '志愿证书']

const volunteerActivities = ref<VolunteerActivity[]>([
  { id: 1, name: '社区关爱老人行', time: '5月10日 09:00-12:00', location: '幸福社区服务中心', hours: 3, people: 24, joined: false },
  { id: 2, name: '城市环保净滩行动', time: '5月12日 08:00-11:00', location: '滨江公园', hours: 3, people: 36, joined: true },
  { id: 3, name: '儿童阅读陪伴', time: '5月14日 14:00-17:00', location: '市图书馆', hours: 3, people: 18, joined: false },
  { id: 4, name: '交通文明劝导', time: '5月16日 07:30-09:00', location: '人民路路口', hours: 1.5, people: 12, joined: false },
  { id: 5, name: '爱心义卖活动', time: '5月18日 10:00-16:00', location: '文化广场', hours: 6, people: 42, joined: false },
  { id: 6, name: '助残日志愿服务', time: '5月20日 09:00-12:00', location: '残疾人服务中心', hours: 3, people: 20, joined: false }
])

const organizations = ref<Organization[]>([
  { id: 1, name: '青年志愿者协会', desc: '组织青年参与社会公益服务', activities: 86, bg: 'linear-gradient(135deg, #4F6EF7 0%, #7B9AFF 100%)', followed: true },
  { id: 2, name: '绿色环保联盟', desc: '推动城市环保与可持续发展', activities: 52, bg: 'linear-gradient(135deg, #2ECC71 0%, #58D68D 100%)', followed: false },
  { id: 3, name: '爱心助学中心', desc: '帮助困难学生完成学业', activities: 38, bg: 'linear-gradient(135deg, #F7854F 0%, #FFB088 100%)', followed: false },
  { id: 4, name: '社区互助会', desc: '促进社区邻里互助与关爱', activities: 64, bg: 'linear-gradient(135deg, #9B59B6 0%, #C39BD3 100%)', followed: false },
  { id: 5, name: '阳光助残协会', desc: '关爱残疾人群体，提供志愿服务', activities: 29, bg: 'linear-gradient(135deg, #4FC7F7 0%, #88D8FF 100%)', followed: true }
])

const certificates = ref<Certificate[]>([
  { id: 1, name: '优秀志愿者证书', org: '市志愿者协会', time: '2025-12-20', no: 'VOL-2025-001286' },
  { id: 2, name: '环保先锋证书', org: '绿色环保联盟', time: '2025-10-15', no: 'ENV-2025-000563' },
  { id: 3, name: '社区服务证书', org: '社区互助会', time: '2025-08-30', no: 'COM-2025-000892' },
  { id: 4, name: '爱心助学证书', org: '爱心助学中心', time: '2025-06-18', no: 'EDU-2025-000341' },
  { id: 5, name: '疫情防控志愿者证书', org: '市卫健委', time: '2025-03-10', no: 'HEA-2025-000775' }
])

const onJoinVolunteer = (item: VolunteerActivity) => {
  if (item.joined) return
  item.joined = true
  item.people++
  uni.showToast({ title: '报名成功', icon: 'success' })
}

const onFollow = (item: Organization) => {
  if (item.followed) return
  item.followed = true
  uni.showToast({ title: '关注成功', icon: 'success' })
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

.volunteer-card {
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.volunteer-name {
  font-size: 32rpx;
  font-weight: 700;
  color: #333333;
}

.volunteer-time {
  font-size: 24rpx;
  color: #666666;
  margin-top: 12rpx;
}

.volunteer-location {
  font-size: 24rpx;
  color: #666666;
  margin-top: 6rpx;
}

.volunteer-meta {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 16rpx;
}

.volunteer-hours {
  font-size: 24rpx;
  color: #4F6EF7;
}

.volunteer-people {
  font-size: 22rpx;
  color: #999999;
}

.volunteer-btn {
  margin-top: 20rpx;
  padding: 14rpx 0;
  background-color: #4F6EF7;
  border-radius: 28rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.volunteer-btn.joined {
  background-color: #E0E0E0;
}

.volunteer-btn-text {
  font-size: 26rpx;
  color: #FFFFFF;
}

.volunteer-btn-text.joined {
  color: #999999;
}

.org-card {
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.org-header {
  display: flex;
  align-items: center;
}

.org-avatar {
  width: 80rpx;
  height: 80rpx;
  border-radius: 50%;
  flex-shrink: 0;
}

.org-info {
  margin-left: 20rpx;
  flex: 1;
}

.org-name {
  font-size: 30rpx;
  font-weight: 700;
  color: #333333;
}

.org-desc {
  font-size: 24rpx;
  color: #999999;
  margin-top: 6rpx;
}

.org-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 20rpx;
}

.org-activities {
  font-size: 24rpx;
  color: #4F6EF7;
}

.follow-btn {
  padding: 10rpx 28rpx;
  background-color: #4F6EF7;
  border-radius: 28rpx;
}

.follow-btn.followed {
  background-color: #E0E0E0;
}

.follow-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
}

.follow-btn-text.followed {
  color: #999999;
}

.cert-card {
  display: flex;
  background-color: #FFFFFF;
  border-radius: 20rpx;
  padding: 28rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.cert-icon {
  font-size: 60rpx;
  margin-right: 24rpx;
  flex-shrink: 0;
}

.cert-info {
  flex: 1;
}

.cert-name {
  font-size: 30rpx;
  font-weight: 700;
  color: #333333;
}

.cert-org {
  font-size: 24rpx;
  color: #666666;
  margin-top: 8rpx;
}

.cert-time {
  font-size: 22rpx;
  color: #999999;
  margin-top: 6rpx;
}

.cert-no {
  font-size: 20rpx;
  color: #CCCCCC;
  margin-top: 6rpx;
}
</style>
