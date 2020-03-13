<template>
  <div class="sc-message-list" ref="scrollList" :style="{backgroundColor: colors.messageList.bg}">
    <template v-if="loading">
      <ContentLoader
        :height="245"
        :width="370"
      >
        <rect x="130" y="0" rx="10" ry="10" width="200" height="120" />
        <rect x="40" y="135" rx="10" ry="10" width="200" height="100" />
      </ContentLoader>
    </template>
    <template v-else-if="!loading && !messages.length">
      <div class="sc-message-list-empty">
        <img width="130" height="130" src="./assets/undraw-messaging.svg" />
        <div class="sc-message-list-empty-heading">
          No Messages
        </div>
        <div class="sc-message-list-empty-sub-heading">
          Send your first message below!
        </div>
      </div>
    </template>
    <template v-else>
      <Message v-for="(message, idx) in messages" :message="message" :chatImageUrl="chatImageUrl(message.author)" :authorName="authorName(message.author)" :key="idx" :colors="colors" :messageStyling="messageStyling" />
      <Message v-show="showTypingIndicator !== ''" :message="{author: showTypingIndicator, type: 'typing'}" :chatImageUrl="chatImageUrl(showTypingIndicator)" :colors="colors" :messageStyling="messageStyling" />
    </template>
  </div>
</template>
<script>
import Message from './Message.vue'
import chatIcon from './assets/chat-icon.svg'
import undrawMessaging from './assets/undraw-messaging.svg'
import { ContentLoader } from 'vue-content-loader'

export default {
  components: {
    UndrawMessagingSVG,
    Message,
    ContentLoader,
  },
  props: {
    participants: {
      type: Array,
      required: true
    },
    messages: {
      type: Array,
      required: true
    },
    showTypingIndicator: {
      type: String,
      required: true
    },
    colors: {
      type: Object,
      required: true
    },
    alwaysScrollToBottom: {
      type: Boolean,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    },
    loading: {
      type: Boolean,
      default: true,
      required: true
    },
  },
  methods: {
    _scrollDown () {
      this.$refs.scrollList.scrollTop = this.$refs.scrollList.scrollHeight
    },
    shouldScrollToBottom() {
      return this.alwaysScrollToBottom || (this.$refs.scrollList.scrollTop > this.$refs.scrollList.scrollHeight - 600)
    },
    profile(author) {
      const profile = this.participants.find(profile => profile.id === author)

      // A profile may not be found for system messages or messages by 'me'
      return profile || {imageUrl: '', name: ''}
    },
    chatImageUrl(author) {
      return this.profile(author).imageUrl
    },
    authorName(author) {
      return this.profile(author).name
    }
  },
  computed: {
    defaultChatIcon() {
      return chatIcon
    },
  },
  mounted () {
    this._scrollDown()
  },
  updated () {
    if (this.shouldScrollToBottom())
      this.$nextTick(this._scrollDown())
  },
}
</script>

<style scoped>
.sc-message-list {
  height: 80%;
  overflow-y: auto;
  background-size: 100%;
  padding: 40px 0px;
}
.sc-message-list-empty {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.sc-message-list-empty-heading {
  margin: 30px 0 0 0;
  color: #9E9DA2;
  font-size: 16px;
  font-weight: bold;
}
.sc-message-list-empty-sub-heading {
  margin: 10px 0 0 0;
  color: #9E9DA2;
  font-size: 14px;
}
</style>
