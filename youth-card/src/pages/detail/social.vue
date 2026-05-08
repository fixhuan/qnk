<template>
  <view class="page">
    <view class="search-bar">
      <view class="search-input-wrap">
        <text class="search-icon">🔍</text>
        <input class="search-input" placeholder="搜索活动、圈子、交友" placeholder-class="search-placeholder" v-model="keyword" />
      </view>
    </view>

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
        <view class="activity-card" v-for="item in filteredActivities" :key="item.id" hover-class="card-hover">
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
        <view class="circle-card" v-for="item in filteredCircles" :key="item.id" hover-class="card-hover">
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
        <view class="friend-card" v-for="item in filteredFriends" :key="item.id" hover-class="card-hover">
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
import { ref, computed } from 'vue'

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
const keyword = ref('')

const activities = ref<Activity[]>([
  { id: 1, name: '周末徒步登山', time: '5月10日 08:00', location: '城郊森林公园', people: 32, bg: '#f0fdf4', joined: false },
  { id: 2, name: '读书分享会', time: '5月11日 14:00', location: '青年活动中心', people: 18, bg: '#eff6ff', joined: true },
  { id: 3, name: '桌游之夜', time: '5月12日 19:00', location: '星空桌游吧', people: 24, bg: '#fef3ee', joined: false },
  { id: 4, name: '摄影采风行', time: '5月15日 09:00', location: '城市文化广场', people: 15, bg: '#fefce8', joined: false },
  { id: 5, name: '飞盘运动趴', time: '5月16日 16:00', location: '体育公园', people: 28, bg: '#f0fdf4', joined: false },
  { id: 6, name: '音乐Live夜', time: '5月18日 20:00', location: 'LiveHouse', people: 45, bg: '#eff6ff', joined: false }
])

const circles = ref<Circle[]>([
  { id: 1, name: '读书爱好者', members: 256, topics: 89, bg: '#eff6ff', joined: true },
  { id: 2, name: '跑步达人', members: 432, topics: 156, bg: '#f0fdf4', joined: false },
  { id: 3, name: '摄影圈', members: 189, topics: 67, bg: '#fef3ee', joined: false },
  { id: 4, name: '美食探店', members: 367, topics: 203, bg: '#fefce8', joined: false },
  { id: 5, name: '编程交流', members: 198, topics: 112, bg: '#eff6ff', joined: true }
])

const friends = ref<Friend[]>([
  { id: 1, name: '小明', interests: ['篮球', '摄影', '旅行'], bg: '#fef3ee', greeted: false },
  { id: 2, name: '小红', interests: ['读书', '瑜伽', '烘焙'], bg: '#f0fdf4', greeted: true },
  { id: 3, name: '阿杰', interests: ['编程', '游戏', '咖啡'], bg: '#eff6ff', greeted: false },
  { id: 4, name: '小美', interests: ['绘画', '音乐', '猫'], bg: '#fefce8', greeted: false },
  { id: 5, name: '大伟', interests: ['健身', '电影', '美食'], bg: '#fef3ee', greeted: false },
  { id: 6, name: '小琳', interests: ['舞蹈', '手工', '旅行'], bg: '#f0fdf4', greeted: false }
])

const filteredActivities = computed(() => {
  return activities.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value) || item.location.includes(keyword.value)
  })
})

const filteredCircles = computed(() => {
  return circles.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value)
  })
})

const filteredFriends = computed(() => {
  return friends.value.filter(item => {
    return !keyword.value || item.name.includes(keyword.value) || item.interests.some(i => i.includes(keyword.value!))
  })
})

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

.banner-wrap {
  padding: 24rpx;
}

.banner {
  border-radius: 12rpx;
  padding: 40rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #fef3ee;
  border: 1rpx solid #f0ebe3;
}

.banner-title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1a1612;
  margin-bottom: 8rpx;
}

.banner-desc {
  font-size: 26rpx;
  color: #6b5e52;
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

.activity-card {
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
  color: #1a1612;
}

.activity-time {
  font-size: 24rpx;
  color: #6b5e52;
  margin-top: 10rpx;
}

.activity-location {
  font-size: 24rpx;
  color: #a89888;
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
  color: #a89888;
}

.join-btn {
  padding: 10rpx 32rpx;
  background: #c2410c;
  border-radius: 8rpx;
}

.join-btn.joined {
  background: #f5f0ea;
}

.join-btn-text {
  font-size: 24rpx;
  color: #ffffff;
}

.join-btn-text.joined {
  color: #a89888;
}

.circle-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
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
  color: #1a1612;
}

.circle-stats {
  display: flex;
  align-items: center;
  margin-top: 8rpx;
}

.circle-stat {
  font-size: 22rpx;
  color: #a89888;
}

.circle-stat-dot {
  font-size: 22rpx;
  color: #e8e0d6;
  margin: 0 8rpx;
}

.friend-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #ffffff;
  border: 1rpx solid #f0ebe3;
  border-radius: 12rpx;
  padding: 24rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 1rpx 4rpx rgba(0,0,0,0.03);
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
  color: #1a1612;
}

.friend-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8rpx;
  margin-top: 10rpx;
}

.friend-tag {
  font-size: 20rpx;
  color: #6b5e52;
  background: #f5f0ea;
  padding: 4rpx 12rpx;
  border-radius: 999rpx;
}

.greet-btn {
  padding: 10rpx 24rpx;
  background: #c2410c;
  border-radius: 8rpx;
  flex-shrink: 0;
}

.greet-btn.greeted {
  background: #f5f0ea;
}

.greet-btn-text {
  font-size: 24rpx;
  color: #ffffff;
}

.greet-btn-text.greeted {
  color: #a89888;
}
</style>
