<template>
  <div class="sc-message-list" ref="scrollList" :style="{backgroundColor: 'transparent'}">
     <transition-group name="list" tag="div">
    <Message :onChooseOption="onChooseOption" v-for="(message, idx) in messages" :nextMessage="messages[idx+1]" :previousMessage="messages[idx-1]"  :message="message" :hasBubblePointer="true" :chatImageUrl="chatImageUrl(message.author)" :authorName="authorName(message.author)" :key="idx" :colors="colors" />
    <Message :key="'typing-message'" v-show="showTypingIndicator !== ''" :hasBubblePointer="true" :message="{author: showTypingIndicator, type: 'typing'}" :chatImageUrl="chatImageUrl(showTypingIndicator)" :colors="colors" />
   </transition-group>
   
  </div>
</template>
<script>
import Message from './Message.vue'
import chatIcon from './assets/chat-icon.svg'

export default {
  components: {
    Message
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
    onChooseOption: {
      type: Function,
      required: false
    }
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
    }
  },
  mounted () {
    this._scrollDown()
  },
  updated () {
    if (this.shouldScrollToBottom())
      this.$nextTick(this._scrollDown())
  }
}
</script>

<style scoped>
.sc-message-list {
  height: 110%;
  overflow-y: auto;
  background-size: 100%;
  padding: 40px 0px;
}
.sc-message-list::-webkit-scrollbar {
  display: none;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
