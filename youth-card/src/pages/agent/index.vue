<template>
  <view class="agent-page">
    <scroll-view
      class="message-list"
      scroll-y
      :scroll-into-view="scrollTarget"
      :scroll-with-animation="true"
    >
      <view
        v-for="(msg, index) in messages"
        :key="index"
        :id="`msg-${index}`"
        class="message-item"
        :class="msg.role === 'user' ? 'message-right' : 'message-left'"
      >
        <view
          class="message-bubble"
          :class="msg.role === 'user' ? 'bubble-user' : 'bubble-ai'"
        >
          <text
            class="message-text"
            :class="msg.role === 'user' ? 'text-user' : 'text-ai'"
          >{{ msg.content }}</text>
        </view>
        <view
          v-if="msg.role === 'ai' && index === 0"
          class="quick-tags"
        >
          <view
            v-for="(tag, tagIndex) in quickTags"
            :key="tagIndex"
            class="quick-tag"
            @tap="onTagTap(tag)"
          >
            <text class="quick-tag-text">{{ tag }}</text>
          </view>
        </view>
      </view>
    </scroll-view>
    <view class="input-area">
      <input
        class="input-box"
        v-model="inputText"
        placeholder="请输入你的问题..."
        confirm-type="send"
        @confirm="sendMessage"
      />
      <view class="send-btn" @tap="sendMessage">
        <text class="send-btn-text">发送</text>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, nextTick } from 'vue'

interface Message {
  role: 'user' | 'ai'
  content: string
}

const messages = ref<Message[]>([
  {
    role: 'ai',
    content: '你好！我是青年卡智能助手，可以为你提供青年政策咨询、租房指南、求职建议等服务。有什么我可以帮你的吗？'
  }
])

const inputText = ref('')
const scrollTarget = ref('')

const quickTags = ref(['青年政策咨询', '租房指南', '求职建议'])

const getAIReply = (userMessage: string): string => {
  if (userMessage.includes('政策')) {
    return '根据最新青年政策，你可以享受以下优惠...'
  }
  if (userMessage.includes('租房') || userMessage.includes('公寓')) {
    return '青年公寓为您提供高性价比的租房选择...'
  }
  if (userMessage.includes('求职') || userMessage.includes('工作')) {
    return '我们为你精选了以下优质岗位...'
  }
  if (userMessage.includes('创业')) {
    return '青年创业支持计划包括...'
  }
  return '感谢你的提问，我正在为你查询相关信息...'
}

const scrollToBottom = () => {
  nextTick(() => {
    scrollTarget.value = ''
    nextTick(() => {
      scrollTarget.value = `msg-${messages.value.length - 1}`
    })
  })
}

const sendMessage = () => {
  const text = inputText.value.trim()
  if (!text) return

  messages.value.push({ role: 'user', content: text })
  inputText.value = ''
  scrollToBottom()

  setTimeout(() => {
    const reply = getAIReply(text)
    messages.value.push({ role: 'ai', content: reply })
    scrollToBottom()
  }, 1000)
}

const onTagTap = (tag: string) => {
  inputText.value = tag
  sendMessage()
}
</script>

<style>
.agent-page {
  display: flex;
  flex-direction: column;
  height: 100vh;
  background-color: #F5F6FA;
}

.message-list {
  flex: 1;
  padding: 24rpx 24rpx 0 24rpx;
  overflow-y: auto;
}

.message-item {
  margin-bottom: 24rpx;
  display: flex;
  flex-direction: column;
}

.message-left {
  align-items: flex-start;
}

.message-right {
  align-items: flex-end;
}

.message-bubble {
  max-width: 75%;
  padding: 20rpx 28rpx;
  border-radius: 16rpx;
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
  line-height: 1.6;
}

.text-user {
  color: #FFFFFF;
}

.text-ai {
  color: #333333;
}

.quick-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 16rpx;
  margin-top: 20rpx;
}

.quick-tag {
  background-color: #EEF1FF;
  border-radius: 32rpx;
  padding: 12rpx 28rpx;
}

.quick-tag-text {
  font-size: 24rpx;
  color: #4F6EF7;
}

.input-area {
  display: flex;
  align-items: center;
  padding: 16rpx 24rpx;
  padding-bottom: calc(16rpx + env(safe-area-inset-bottom));
  background-color: #FFFFFF;
  border-top: 1rpx solid #EEEEEE;
}

.input-box {
  flex: 1;
  height: 72rpx;
  background-color: #F5F6FA;
  border-radius: 36rpx;
  padding: 0 28rpx;
  font-size: 28rpx;
  color: #333333;
}

.send-btn {
  margin-left: 16rpx;
  background-color: #4F6EF7;
  border-radius: 36rpx;
  height: 72rpx;
  padding: 0 32rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.send-btn-text {
  color: #FFFFFF;
  font-size: 28rpx;
}
</style>
