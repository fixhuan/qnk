<template>
  <view class="page">
    <view class="banner-wrap">
      <view class="banner">
        <text class="banner-title">青年社交季</text>
        <text class="banner-desc">认识新朋友，发现新世界</text>
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
      <template v-if="currentCategory === '活动'">
        <view class="activity-card" v-for="item in activities" :key="item.id" hover-class="card-hover">
          <view class="activity-cover" :style="{ background: item.bg }"></view>
          <view class="activity-info">
            <text class="activity-name">{{ item.name }}</text>
            <text class="activity-time">{{ item.time }}</text>
            <text class="activity-location">{{ item.location }}</text>
            <view class="activity-bottom">
              <text class="activity-people">{{ item.people }}人参与</text>
              <view class="join-btn" :class="{ joined: item.joined }" @click="onJoinActivity(item)">
                <text class="join-btn-text" :class="{ joined: item.joined }">{{ item.joined ? '已报名' : '报名' }}</text>
              </view>
            </view>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '圈子'">
        <view class="circle-card" v-for="item in circles" :key="item.id" hover-class="card-hover">
          <view class="circle-info">
            <view class="circle-avatar" :style="{ background: item.bg }"></view>
            <view class="circle-detail">
              <text class="circle-name">{{ item.name }}</text>
              <view class="circle-stats">
                <text class="circle-stat">{{ item.members }}成员</text>
                <text class="circle-stat-dot">·</text>
                <text class="circle-stat">{{ item.topics }}话题</text>
              </view>
            </view>
          </view>
          <view class="join-btn" :class="{ joined: item.joined }" @click="onJoinCircle(item)">
            <text class="join-btn-text" :class="{ joined: item.joined }">{{ item.joined ? '已加入' : '加入' }}</text>
          </view>
        </view>
      </template>

      <template v-if="currentCategory === '交友'">
        <view class="friend-card" v-for="item in friends" :key="item.id" hover-class="card-hover">
          <view class="friend-info">
            <view class="friend-avatar" :style="{ background: item.bg }"></view>
            <view class="friend-detail">
              <text class="friend-name">{{ item.name }}</text>
              <view class="friend-tags">
                <text class="friend-tag" v-for="tag in item.interests" :key="tag">{{ tag }}</text>
              </view>
            </view>
          </view>
          <view class="greet-btn" :class="{ greeted: item.greeted }" @click="onGreet(item)">
            <text class="greet-btn-text" :class="{ greeted: item.greeted }">{{ item.greeted ? '已打招呼' : '打招呼' }}</text>
          </view>
        </view>
      </template>
    </scroll-view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Activity {
  id: number
  name: string
  time: string
  location: string
  people: number
  bg: string
  joined: boolean
}

interface Circle {
  id: number
  name: string
  members: number
  topics: number
  bg: string
  joined: boolean
}

interface Friend {
  id: number
  name: string
  interests: string[]
  bg: string
  greeted: boolean
}

const currentCategory = ref('活动')
const categories = ['活动', '圈子', '交友']

const activities = ref<Activity[]>([
  { id: 1, name: '周末徒步登山', time: '5月10日 08:00', location: '城郊森林公园', people: 32, bg: 'rgba(52,199,89,0.08)', joined: false },
  { id: 2, name: '读书分享会', time: '5月11日 14:00', location: '青年活动中心', people: 18, bg: 'rgba(90,200,250,0.08)', joined: true },
  { id: 3, name: '桌游之夜', time: '5月12日 19:00', location: '星空桌游吧', people: 24, bg: 'rgba(65,108,129,0.08)', joined: false },
  { id: 4, name: '摄影采风行', time: '5月15日 09:00', location: '城市文化广场', people: 15, bg: 'rgba(255,149,0,0.08)', joined: false },
  { id: 5, name: '飞盘运动趴', time: '5月16日 16:00', location: '体育公园', people: 28, bg: 'rgba(52,199,89,0.08)', joined: false },
  { id: 6, name: '音乐Live夜', time: '5月18日 20:00', location: 'LiveHouse', people: 45, bg: 'rgba(90,200,250,0.08)', joined: false }
])

const circles = ref<Circle[]>([
  { id: 1, name: '读书爱好者', members: 256, topics: 89, bg: 'rgba(90,200,250,0.08)', joined: true },
  { id: 2, name: '跑步达人', members: 432, topics: 156, bg: 'rgba(52,199,89,0.08)', joined: false },
  { id: 3, name: '摄影圈', members: 189, topics: 67, bg: 'rgba(65,108,129,0.08)', joined: false },
  { id: 4, name: '美食探店', members: 367, topics: 203, bg: 'rgba(255,149,0,0.08)', joined: false },
  { id: 5, name: '编程交流', members: 198, topics: 112, bg: 'rgba(90,200,250,0.08)', joined: true }
])

const friends = ref<Friend[]>([
  { id: 1, name: '小明', interests: ['篮球', '摄影', '旅行'], bg: 'rgba(65,108,129,0.08)', greeted: false },
  { id: 2, name: '小红', interests: ['读书', '瑜伽', '烘焙'], bg: 'rgba(52,199,89,0.08)', greeted: true },
  { id: 3, name: '阿杰', interests: ['编程', '游戏', '咖啡'], bg: 'rgba(90,200,250,0.08)', greeted: false },
  { id: 4, name: '小美', interests: ['绘画', '音乐', '猫'], bg: 'rgba(255,149,0,0.08)', greeted: false },
  { id: 5, name: '大伟', interests: ['健身', '电影', '美食'], bg: 'rgba(65,108,129,0.08)', greeted: false },
  { id: 6, name: '小琳', interests: ['舞蹈', '手工', '旅行'], bg: 'rgba(52,199,89,0.08)', greeted: false }
])

const onJoinActivity = (item: Activity) => {
  if (item.joined) return
  item.joined = true
  item.people++
  uni.showToast({ title: '报名成功', icon: 'success' })
}

const onJoinCircle = (item: Circle) => {
  if (item.joined) return
  item.joined = true
  item.members++
  uni.showToast({ title: '加入成功', icon: 'success' })
}

const onGreet = (item: Friend) => {
  if (item.greeted) return
  item.greeted = true
  uni.showToast({ title: '打招呼成功', icon: 'success' })
}
</script>

<style>
.page {
  min-height: 100vh;
  background-color: #F1F3F4;
  display: flex;
  flex-direction: column;
}

.banner-wrap {
  padding: 24rpx;
}

.banner {
  border-radius: 16rpx;
  padding: 40rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: rgba(65,108,129,0.08);
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
}

.banner-title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1C1C1E;
  margin-bottom: 8rpx;
}

.banner-desc {
  font-size: 26rpx;
  color: #8E8E93;
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
  background: #F8F9FA;
  margin-right: 16rpx;
}

.category-tag.active {
  background: #416C81;
}

.category-text {
  font-size: 26rpx;
  color: #8E8E93;
}

.category-text.active-text {
  color: #FFFFFF;
  font-weight: 600;
}

.list-scroll {
  flex: 1;
  padding: 20rpx 24rpx;
}

.activity-card {
  background: #FFFFFF;
  border-radius: 16rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
}

.card-hover {
  background: #F8F9FA;
}

.activity-cover {
  width: 100%;
  height: 240rpx;
  border-radius: 12rpx;
}

.activity-info {
  margin-top: 20rpx;
}

.activity-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #1C1C1E;
}

.activity-time {
  font-size: 24rpx;
  color: #8E8E93;
  margin-top: 10rpx;
}

.activity-location {
  font-size: 24rpx;
  color: #AEAEB2;
  margin-top: 6rpx;
}

.activity-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 16rpx;
}

.activity-people {
  font-size: 24rpx;
  color: #AEAEB2;
}

.join-btn {
  padding: 10rpx 32rpx;
  background: #416C81;
  border-radius: 12rpx;
}

.join-btn.joined {
  background: #F8F9FA;
}

.join-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
}

.join-btn-text.joined {
  color: #8E8E93;
}

.circle-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #FFFFFF;
  border-radius: 16rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
}

.circle-info {
  display: flex;
  align-items: center;
  flex: 1;
}

.circle-avatar {
  width: 88rpx;
  height: 88rpx;
  border-radius: 50%;
  flex-shrink: 0;
}

.circle-detail {
  margin-left: 20rpx;
}

.circle-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #1C1C1E;
}

.circle-stats {
  display: flex;
  align-items: center;
  margin-top: 8rpx;
}

.circle-stat {
  font-size: 22rpx;
  color: #AEAEB2;
}

.circle-stat-dot {
  font-size: 22rpx;
  color: #E5E5EA;
  margin: 0 8rpx;
}

.friend-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #FFFFFF;
  border-radius: 16rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 2rpx 16rpx rgba(0,0,0,0.04), 0 0 1rpx rgba(0,0,0,0.1);
}

.friend-info {
  display: flex;
  align-items: center;
  flex: 1;
}

.friend-avatar {
  width: 88rpx;
  height: 88rpx;
  border-radius: 50%;
  flex-shrink: 0;
}

.friend-detail {
  margin-left: 20rpx;
  flex: 1;
}

.friend-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #1C1C1E;
}

.friend-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8rpx;
  margin-top: 10rpx;
}

.friend-tag {
  font-size: 20rpx;
  color: #8E8E93;
  background: #F8F9FA;
  padding: 4rpx 12rpx;
  border-radius: 999rpx;
}

.greet-btn {
  padding: 10rpx 24rpx;
  background: #416C81;
  border-radius: 12rpx;
  flex-shrink: 0;
}

.greet-btn.greeted {
  background: #F8F9FA;
}

.greet-btn-text {
  font-size: 24rpx;
  color: #FFFFFF;
}

.greet-btn-text.greeted {
  color: #8E8E93;
}
</style>
