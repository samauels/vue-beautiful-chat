<template>
  <div class="sc-message">
    <div class="sc-message--content" :class="{
        sent: message.author === 'me',
        received: message.author !== 'me' && message.type !== 'system',
        system: message.type === 'system',
        showBubble:(nextMessage &&message.author!==nextMessage.author) 
      }">
      <div v-if="message.type !== 'system'" :title="authorName" class="sc-message--avatar" :style="{
        backgroundImage: `url(${chatImageUrl})`
      }" v-tooltip="message.author"></div>
      <TextMessage v-if="message.type === 'text'" :data="message.data" :messageColors="determineMessageColors()" />
      <EmojiMessage v-else-if="message.type === 'emoji'" :data="message.data" />
      <FileMessage v-else-if="message.type === 'file'" :data="message.data" :messageColors="determineMessageColors()" />
      <TypingMessage v-else-if="message.type === 'typing'" :messageColors="determineMessageColors()" />
      <SystemMessage v-else-if="message.type === 'system'" :data="message.data" :messageColors="determineMessageColors()" />
      <SelectMessage v-else-if="message.type === 'select'" :onChooseOption="onChooseOption" :data="message.data" :messageColors="determineMessageColors()" />
    </div>
  </div>
</template>

<script>
import TextMessage from './TextMessage.vue'
import FileMessage from './FileMessage.vue'
import EmojiMessage from './EmojiMessage.vue'
import TypingMessage from './TypingMessage.vue'
import SystemMessage from './SystemMessage.vue'
import SelectMessage from './SelectMessage.vue'
import chatIcon from './assets/chat-icon.svg'

export default {
  data () {
    return {

    }
  },
  components: {
    TextMessage,
    FileMessage,
    EmojiMessage,
    TypingMessage,
    SystemMessage,
    SelectMessage
  },
  props: {
    message: {
      type: Object,
      required: true
    },
    chatImageUrl: {
      type: String,
      default: chatIcon
    },
    colors: {
      type: Object,
      required: true
    },
    authorName: {
      type: String
    },
    hasBubblePointer:{
      type: Boolean
    },
    onChooseOption: {
      type: Function,
      required: false
    },
    nextMessage: {
    },
    previousMessage: {
    }
  },
  methods: {
    sentColorsStyle() {
      return {
        color: this.colors.sentMessage.text,
        backgroundColor: this.colors.sentMessage.bg
      }
    },
    receivedColorsStyle() {
      return {
        color: this.colors.receivedMessage.text,
        backgroundColor: this.colors.receivedMessage.bg
      }
    },
    determineMessageColors() {
      return this.message.author === 'me' ? this.sentColorsStyle() : this.receivedColorsStyle()
    }
  }
}
</script>
<style lang="scss">
.sc-message {
  width: 80%;
  margin: auto;
  padding-bottom: 10px;
  display: flex;
}

.sc-message--content {
  width: 100%;
  display: flex;
}

.sc-message--content.sent {
  justify-content: flex-end;
}

.sc-message--content.system {
  justify-content: center;
}

.sc-message--content.sent .sc-message--avatar {
  display: none;
}

.sc-message--avatar {
  background-repeat: no-repeat;
  background-size: 100%;
  background-position: center;
  min-width: 30px;
  min-height: 30px;
  border-radius: 50%;
  align-self: center;
  margin-right: 15px;
}

.sc-message--meta {
  font-size: xx-small;
  margin-bottom: 0px;
  color: white;
  text-align: center;
}

@media (max-width: 450px) {
  .sc-message {
    width: 80%;
  }
}

.sc-message--text {
  padding: 5px 20px;
  border-radius: 6px;
  font-weight: 300;
  font-size: 14px;
  line-height: 1.4;
  /* white-space: pre-wrap; */
  -webkit-font-smoothing: subpixel-antialiased
}
.sc-message--content.sent .sc-message--text {
  color: white;
  background-color: #4e8cff;
  max-width: calc(100% - 120px);
  word-wrap: break-word;
  display: inline-block;
  margin: 0.5em;
  min-height: 2em;
  padding: 0.5em 1.5em;
  position: relative;
  border-radius: 1em;
  line-height: 1.5;
}
.sc-message--content.sent.showBubble .sc-message--text::before {
   content: '';
  display: block;
  width: 0.4em;
  height: 1em;
  position: absolute;
  right: -0.4em;
  bottom: 0;
  border-left: 1em solid #4e8cff;
  border-bottom-left-radius: 100%;
  z-index: -1;
}

.sc-message--content.received .sc-message--text {
  color: #263238;
  background-color: #18d080;
  margin-right: 40px;
  display: inline-block;
  margin: 0.5em;
  min-height: 2em;
  padding: 0.5em 1.5em;
  position: relative;
  border-radius: 1em;
  line-height: 1.5;
}
.sc-message--content.received.showBubble .sc-message--text::before {
   content: '';
  display: block;
  width: 0.5em;
  height: 1em;
  position: absolute;
  left: -0.4em;
  bottom: 0;
  border-right: 1em solid #18d080;
  border-bottom-right-radius: 100%;
  z-index: -1;
}

.tooltip {
  display: block !important;
  z-index: 10000;
  .tooltip-inner {
    background: black;
    color: white;
    border-radius: 16px;
    padding: 5px 10px 4px;
  }
  .tooltip-arrow {
    width: 0;
    height: 0;
    border-style: solid;
    position: absolute;
    margin: 5px;
    border-color: black;
    z-index: 1;
  }
  &[x-placement^="top"] {
    margin-bottom: 5px;
    .tooltip-arrow {
      border-width: 5px 5px 0 5px;
      border-left-color: transparent !important;
      border-right-color: transparent !important;
      border-bottom-color: transparent !important;
      bottom: -5px;
      left: calc(50% - 5px);
      margin-top: 0;
      margin-bottom: 0;
    }
  }
  &[x-placement^="bottom"] {
    margin-top: 5px;
    .tooltip-arrow {
      border-width: 0 5px 5px 5px;
      border-left-color: transparent !important;
      border-right-color: transparent !important;
      border-top-color: transparent !important;
      top: -5px;
      left: calc(50% - 5px);
      margin-top: 0;
      margin-bottom: 0;
    }
  }
  &[x-placement^="right"] {
    margin-left: 5px;
    .tooltip-arrow {
      border-width: 5px 5px 5px 0;
      border-left-color: transparent !important;
      border-top-color: transparent !important;
      border-bottom-color: transparent !important;
      left: -5px;
      top: calc(50% - 5px);
      margin-left: 0;
      margin-right: 0;
    }
  }
  &[x-placement^="left"] {
    margin-right: 5px;
    .tooltip-arrow {
      border-width: 5px 0 5px 5px;
      border-top-color: transparent !important;
      border-right-color: transparent !important;
      border-bottom-color: transparent !important;
      right: -5px;
      top: calc(50% - 5px);
      margin-left: 0;
      margin-right: 0;
    }
  }
  &[aria-hidden='true'] {
    visibility: hidden;
    opacity: 0;
    transition: opacity .15s, visibility .15s;
  }
  &[aria-hidden='false'] {
    visibility: visible;
    opacity: 1;
    transition: opacity .15s;
  }
  &.info {
    $color: rgba(#004499, .9);
    .tooltip-inner {
      background: $color;
      color: white;
      padding: 24px;
      border-radius: 5px;
      box-shadow: 0 5px 30px rgba(black, .1);
    }
    .tooltip-arrow {
      border-color: $color;
    }
  }
  &.popover {
    $color: #f9f9f9;
    .popover-inner {
      background: $color;
      color: black;
      padding: 24px;
      border-radius: 5px;
      box-shadow: 0 5px 30px rgba(black, .1);
    }
    .popover-arrow {
      border-color: $color;
    }
  }
}
</style>
