<template>
  <view class="agent-page">
    <view class="ai-header">
      <view class="ai-avatar">
        <text class="ai-avatar-icon">🤖</text>
      </view>
      <view class="ai-info">
        <text class="ai-name">青年卡AI助手</text>
        <view class="ai-status">
          <view class="status-dot"></view>
          <text class="status-text">在线</text>
        </view>
      </view>
    </view>

    <scroll-view class="func-scroll" scroll-x :show-scrollbar="false">
      <view class="func-list">
        <view
          v-for="(func, index) in funcList"
          :key="index"
          class="func-card"
          hover-class="func-card-active"
          @tap="onFuncTap(func)"
        >
          <text class="func-icon">{{ func.icon }}</text>
          <text class="func-name">{{ func.name }}</text>
        </view>
      </view>
    </scroll-view>

    <scroll-view
      class="message-list"
      scroll-y
      :scroll-into-view="scrollTarget"
      :scroll-with-animation="true"
    >
      <view class="quick-tags-scroll">
        <scroll-view scroll-x :show-scrollbar="false">
          <view class="quick-tags">
            <view
              v-for="(tag, index) in quickTags"
              :key="index"
              class="quick-tag"
              hover-class="quick-tag-active"
              @tap="onTagTap(tag)"
            >
              <text class="quick-tag-text">{{ tag }}</text>
            </view>
          </view>
        </scroll-view>
      </view>

      <view
        v-for="(msg, index) in messages"
        :key="index"
        :id="`msg-${index}`"
        class="message-item"
        :class="msg.role === 'user' ? 'message-right' : 'message-left'"
      >
        <view v-if="msg.role === 'ai'" class="msg-avatar">
          <text class="msg-avatar-icon">🤖</text>
        </view>
        <view class="msg-content">
          <view
            class="message-bubble"
            :class="msg.role === 'user' ? 'bubble-user' : 'bubble-ai'"
          >
            <text
              class="message-text"
              :class="msg.role === 'user' ? 'text-user' : 'text-ai'"
            >{{ msg.content }}</text>
          </view>
          <text class="message-time">{{ msg.time }}</text>
        </view>
      </view>

      <view v-if="isTyping" class="message-item message-left">
        <view class="msg-avatar">
          <text class="msg-avatar-icon">🤖</text>
        </view>
        <view class="msg-content">
          <view class="message-bubble bubble-ai typing-bubble">
            <view class="typing-indicator">
              <view class="typing-dot dot1"></view>
              <view class="typing-dot dot2"></view>
              <view class="typing-dot dot3"></view>
            </view>
          </view>
        </view>
      </view>

      <view id="msg-bottom" style="height: 20rpx;"></view>
    </scroll-view>

    <view class="input-area">
      <view class="input-wrapper">
        <input
          class="input-box"
          v-model="inputText"
          placeholder="请输入你的问题..."
          confirm-type="send"
          @confirm="sendMessage"
        />
        <view class="voice-btn" hover-class="voice-btn-active" @tap="onVoiceTap">
          <text class="voice-icon">🎤</text>
        </view>
      </view>
      <view class="send-btn" hover-class="send-btn-active" @tap="sendMessage">
        <text class="send-icon">➤</text>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, nextTick } from 'vue'

interface Message {
  role: 'user' | 'ai'
  content: string
  time: string
}

const funcList = ref([
  { icon: '📋', name: '政策查询', keyword: '政策' },
  { icon: '🏠', name: '租房助手', keyword: '租房' },
  { icon: '💼', name: '求职指导', keyword: '求职' },
  { icon: '🎓', name: '学习规划', keyword: '学习' }
])

const quickTags = ref([
  '青年补贴政策',
  '租房补贴申请',
  '简历优化建议',
  '面试技巧',
  '创业贷款政策',
  '夜校课程推荐',
  '青年公寓申请',
  '志愿者注册'
])

const getCurrentTime = () => {
  const now = new Date()
  const h = now.getHours().toString().padStart(2, '0')
  const m = now.getMinutes().toString().padStart(2, '0')
  return `${h}:${m}`
}

const messages = ref<Message[]>([
  {
    role: 'ai',
    content: '你好！我是青年卡AI助手，可以为你提供青年政策咨询、租房指南、求职建议、学习规划等服务。有什么我可以帮你的吗？',
    time: getCurrentTime()
  }
])

const inputText = ref('')
const scrollTarget = ref('')
const isTyping = ref(false)

const getAIReply = (userMessage: string): string => {
  if (userMessage.includes('政策') || userMessage.includes('补贴')) {
    return '根据最新青年政策，你可以享受以下三项优惠：\n\n1. 青年租房补贴：月收入低于8000元的35岁以下青年，可申请每月最高1500元的租房补贴，需提供租赁合同和收入证明，到所在社区服务中心申请。\n\n2. 创业扶持补贴：首次创业的青年可申请最高5万元的一次性创业补贴，同时享受3年税收减免政策，需在创业后6个月内向人社局提交申请。\n\n3. 技能培训补贴：参加政府认可的职业技能培训，可获得培训费用80%的补贴，每人每年最高补贴3000元，通过"青年卡"APP在线报名即可。'
  }
  if (userMessage.includes('租房') || userMessage.includes('公寓')) {
    return '关于租房，我为你整理了以下信息：\n\n1. 推荐区域：高新区青年社区交通便利，地铁直达；大学城周边生活配套齐全，性价比高；开发区新城区房源较新，环境优美。\n\n2. 价格范围：单间月租800-1500元，一室一厅1500-2500元，合租人均600-1200元。青年公寓有政府补贴，价格比市场低20%-30%。\n\n3. 注意事项：签合同前确认房东身份和房产证，仔细检查房屋设施并拍照留存，了解物业费和水电费标准，建议通过青年卡平台签约更有保障。'
  }
  if (userMessage.includes('求职') || userMessage.includes('工作') || userMessage.includes('简历') || userMessage.includes('面试')) {
    return '关于求职，我有以下建议：\n\n1. 行业趋势：目前互联网、新能源、生物医药、人工智能等领域对青年人才需求旺盛，薪资待遇也较有竞争力，建议重点关注这些方向。\n\n2. 简历建议：突出项目经验和量化成果，用数据说话；针对不同岗位调整简历重点；保持简历简洁在一页以内；使用青年卡简历模板可获得HR优先筛选。\n\n3. 面试技巧：提前了解公司业务和岗位要求；准备3-5个有深度的问题反问面试官；用STAR法则（情境-任务-行动-结果）组织回答；面试后24小时内发送感谢信。'
  }
  if (userMessage.includes('创业') || userMessage.includes('贷款')) {
    return '关于创业支持，以下是详细信息：\n\n1. 扶持政策：青年创业可申请最高50万元的创业担保贷款，3年内免息；首次创业成功可获5万元一次性补贴；入驻政府孵化器可享1-2年免租期。\n\n2. 孵化器信息：市青年创业孵化基地提供办公场地、导师辅导、法律咨询等一站式服务，目前入驻率85%，建议尽早申请。高新区数字产业孵化器专注科技类项目，配套完善。\n\n3. 融资渠道：除银行贷款外，可关注青年创业基金、天使投资人网络、众筹平台等。青年卡平台定期举办创业路演活动，帮助对接投资机构。'
  }
  if (userMessage.includes('学习') || userMessage.includes('课程') || userMessage.includes('夜校') || userMessage.includes('规划')) {
    return '关于学习规划，我为你推荐以下内容：\n\n1. 课程推荐：夜校开设了Python编程、UI设计、新媒体运营、英语口语等热门课程，每周2-3次晚间授课，不影响白天工作。线上平台还有500+免费课程可供选择。\n\n2. 学习路径：建议先确定职业方向，再选择对应技能树。如互联网方向：基础办公→数据分析→专业领域深耕；设计方向：设计基础→工具精通→作品集打造。\n\n3. 时间规划：建议每天固定1-2小时学习时间，利用碎片时间复习。设置阶段性目标，每完成一个阶段给自己奖励。青年卡学习打卡功能可帮你养成习惯，连续打卡还有积分奖励。'
  }
  if (userMessage.includes('志愿') || userMessage.includes('志愿者')) {
    return '关于志愿者注册，以下是相关信息：\n\n1. 注册流程：通过青年卡APP"志愿者专区"在线注册，填写个人信息和技能特长，选择感兴趣的志愿服务类别，完成线上培训后即可参与活动。\n\n2. 志愿类型：社区服务、环保公益、教育帮扶、大型活动保障等，每类活动都有专业培训，确保服务质量。\n\n3. 权益保障：志愿服务时长可兑换青年卡积分，年度优秀志愿者可获得表彰和额外福利，包括优先申请青年公寓、免费参加培训课程等。'
  }
  return '感谢你的提问！我是青年卡AI助手，可以为你提供以下方面的帮助：\n\n📋 青年政策咨询和补贴申请指导\n🏠 租房建议和青年公寓推荐\n💼 求职指导和简历优化建议\n🎓 学习规划和课程推荐\n🚀 创业扶持和融资渠道信息\n🤝 志愿者活动注册和参与\n\n你可以直接点击上方的功能卡片或快捷标签，快速获取相关信息哦！'
}

const scrollToBottom = () => {
  nextTick(() => {
    scrollTarget.value = ''
    nextTick(() => {
      scrollTarget.value = 'msg-bottom'
    })
  })
}

const sendMessage = () => {
  const text = inputText.value.trim()
  if (!text) return

  messages.value.push({ role: 'user', content: text, time: getCurrentTime() })
  inputText.value = ''
  scrollToBottom()

  isTyping.value = true
  scrollToBottom()

  setTimeout(() => {
    isTyping.value = false
    const reply = getAIReply(text)
    messages.value.push({ role: 'ai', content: reply, time: getCurrentTime() })
    scrollToBottom()
  }, 1500)
}

const onTagTap = (tag: string) => {
  inputText.value = tag
  sendMessage()
}

const onFuncTap = (func: { icon: string; name: string; keyword: string }) => {
  inputText.value = func.name
  sendMessage()
}

const onVoiceTap = () => {
  uni.showToast({ title: '语音功能开发中', icon: 'none' })
}
</script>

<style scoped>
.agent-page {
  display: flex;
  flex-direction: column;
  height: 100vh;
  background-color: #F5F6FA;
}

.ai-header {
  display: flex;
  align-items: center;
  padding: 20rpx 32rpx;
  background-color: #FFFFFF;
}

.ai-avatar {
  width: 60rpx;
  height: 60rpx;
  border-radius: 50%;
  background: linear-gradient(135deg, #4F6EF7, #7B93FA);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.ai-avatar-icon {
  font-size: 32rpx;
}

.ai-info {
  margin-left: 20rpx;
  display: flex;
  flex-direction: column;
}

.ai-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
}

.ai-status {
  display: flex;
  align-items: center;
  margin-top: 4rpx;
}

.status-dot {
  width: 12rpx;
  height: 12rpx;
  border-radius: 50%;
  background-color: #4CD964;
  margin-right: 8rpx;
}

.status-text {
  font-size: 22rpx;
  color: #4CD964;
}

.func-scroll {
  background-color: #FFFFFF;
  padding-bottom: 20rpx;
  border-bottom: 1rpx solid #F0F0F5;
}

.func-list {
  display: flex;
  padding: 0 24rpx;
  gap: 20rpx;
}

.func-card {
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 150rpx;
  height: 120rpx;
  background-color: #F5F6FA;
  border-radius: 20rpx;
}

.func-card-active {
  background-color: #EEF1FF;
  transform: scale(0.96);
}

.func-icon {
  font-size: 40rpx;
  margin-bottom: 8rpx;
}

.func-name {
  font-size: 22rpx;
  color: #333333;
}

.message-list {
  flex: 1;
  padding: 0 24rpx;
  overflow-y: auto;
}

.quick-tags-scroll {
  padding: 20rpx 0;
}

.quick-tags {
  display: flex;
  gap: 16rpx;
  padding-right: 24rpx;
}

.quick-tag {
  flex-shrink: 0;
  background-color: #EEF1FF;
  border-radius: 32rpx;
  padding: 12rpx 28rpx;
}

.quick-tag-active {
  background-color: #4F6EF7;
}

.quick-tag-active .quick-tag-text {
  color: #FFFFFF;
}

.quick-tag-text {
  font-size: 24rpx;
  color: #4F6EF7;
  white-space: nowrap;
}

.message-item {
  margin-bottom: 24rpx;
  display: flex;
  align-items: flex-start;
}

.message-left {
  flex-direction: row;
}

.message-right {
  flex-direction: row-reverse;
}

.msg-avatar {
  width: 60rpx;
  height: 60rpx;
  border-radius: 50%;
  background: linear-gradient(135deg, #4F6EF7, #7B93FA);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.msg-avatar-icon {
  font-size: 28rpx;
}

.msg-content {
  max-width: 70%;
  margin: 0 16rpx;
  display: flex;
  flex-direction: column;
}

.message-right .msg-content {
  align-items: flex-end;
}

.message-bubble {
  padding: 20rpx 28rpx;
  border-radius: 20rpx;
  word-break: break-all;
}

.bubble-user {
  background-color: #4F6EF7;
  border-top-right-radius: 4rpx;
}

.bubble-ai {
  background-color: #FFFFFF;
  border-top-left-radius: 4rpx;
}

.message-text {
  font-size: 28rpx;
  line-height: 1.7;
}

.text-user {
  color: #FFFFFF;
}

.text-ai {
  color: #333333;
}

.message-time {
  font-size: 20rpx;
  color: #999999;
  margin-top: 8rpx;
}

.typing-bubble {
  padding: 24rpx 32rpx;
}

.typing-indicator {
  display: flex;
  align-items: center;
  gap: 8rpx;
}

.typing-dot {
  width: 12rpx;
  height: 12rpx;
  border-radius: 50%;
  background-color: #999999;
}

.dot1 {
  animation: typingBounce 1.2s ease-in-out infinite;
}

.dot2 {
  animation: typingBounce 1.2s ease-in-out 0.2s infinite;
}

.dot3 {
  animation: typingBounce 1.2s ease-in-out 0.4s infinite;
}

@keyframes typingBounce {
  0%, 60%, 100% {
    transform: translateY(0);
    opacity: 0.4;
  }
  30% {
    transform: translateY(-10rpx);
    opacity: 1;
  }
}

.input-area {
  display: flex;
  align-items: center;
  padding: 16rpx 24rpx;
  padding-bottom: calc(16rpx + env(safe-area-inset-bottom));
  background-color: #FFFFFF;
  box-shadow: 0 -4rpx 20rpx rgba(0, 0, 0, 0.05);
}

.input-wrapper {
  flex: 1;
  display: flex;
  align-items: center;
  background-color: #F5F6FA;
  border-radius: 36rpx;
  padding: 0 16rpx;
  height: 72rpx;
}

.input-box {
  flex: 1;
  height: 72rpx;
  font-size: 28rpx;
  color: #333333;
  background-color: transparent;
}

.voice-btn {
  width: 56rpx;
  height: 56rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
}

.voice-btn-active {
  background-color: #E8E8E8;
}

.voice-icon {
  font-size: 32rpx;
}

.send-btn {
  margin-left: 16rpx;
  width: 72rpx;
  height: 72rpx;
  background-color: #4F6EF7;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.send-btn-active {
  background-color: #3A56D4;
  transform: scale(0.95);
}

.send-icon {
  color: #FFFFFF;
  font-size: 32rpx;
}
</style>
