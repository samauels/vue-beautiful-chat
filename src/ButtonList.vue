<template>
  <div>
    <div>
        <div class="col-lg-6" style="    display: inline-block;
    width: 49%;"><button style="width:100%;    padding: 20px 25px;" :style="{'backgroundColor':colors.sentMessage.bg,'color': colors.sentMessage.text}">Yes</button></div>
        <div class="col-lg-6" style="    display: inline-block;
    width: 49%;"><button style="width:100%;    padding: 20px 25px;" :style="{'backgroundColor':colors.sentMessage.bg,'color': colors.sentMessage.text}">No</button></div>
    </div>
  </div>
</template>


<script>

export default {
  components: {
    
  },
  props: {
   colors: {
      type: Object,
      required: true
    },
    buttonData: {
      type: Object,
      required: false
    }
  },
  data () {
    return {
      file: null,
      inputActive: false
    }
  },
  methods: {
    cancelFile () {
      this.file = null
    },
    setInputActive (onoff) {
      this.inputActive = onoff
    },
    handleKey (event) {
      if (event.keyCode === 13 && !event.shiftKey) {
        this._submitText(event)
        event.preventDefault()
      }
    },
    _submitText (event) {
      const text = this.$refs.userInput.textContent
      const file = this.file
      if (file) {
        if (text && text.length > 0) {
          this.onSubmit({
            author: 'me',
            type: 'file',
            data: { text, file }
          })
          this.file = null
          this.$refs.userInput.innerHTML = ''
        } else {
          this.onSubmit({
            author: 'me',
            type: 'file',
            data: { file }
          })
          this.file = null
        }
      } else {
        if (text && text.length > 0) {
          this.onSubmit({
            author: 'me',
            type: 'text',
            data: { text }
          })
          this.$refs.userInput.innerHTML = ''
        }
      }
    },
    _handleEmojiPicked (emoji) {
      this.onSubmit({
        author: 'me',
        type: 'emoji',
        data: { emoji }
      })
    },
    _handleFileSubmit (file) {
      this.file = file
    }
  }
}
</script>

<style>
.sc-user-input {
  min-height: 55px;
  margin: 0px;
  position: relative;
  bottom: 0;
  display: flex;
  background-color: #f4f7f9;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  transition: background-color .2s ease,box-shadow .2s ease;
}

.sc-user-input--text {
  width: 300px;
  resize: none;
  border: none;
  outline: none;
  border-bottom-left-radius: 10px;
  box-sizing: border-box;
  padding: 18px;
  font-size: 15px;
  font-weight: 400;
  line-height: 1.33;
  white-space: pre-wrap;
  word-wrap: break-word;
  color: #565867;
  -webkit-font-smoothing: antialiased;
  max-height: 200px;
  overflow: scroll;
  bottom: 0;
  overflow-x: hidden;
  overflow-y: auto;
}

.sc-user-input--text:empty:before {
  content: attr(placeholder);
  display: block; /* For Firefox */
  /* color: rgba(86, 88, 103, 0.3); */
  filter: contrast(15%);
  outline: none;
}

.sc-user-input--buttons {
  width: 100px;
  position: absolute;
  right: 30px;
  height: 100%;
  display: flex;
}

.sc-user-input--button:first-of-type {
  width: 40px;
}

.sc-user-input--button {
  width: 30px;
  height: 55px;
  margin-left: 2px;
  margin-right: 2px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.sc-user-input.active {
  box-shadow: none;
  background-color: white;
  box-shadow: 0px -5px 20px 0px rgba(150, 165, 190, 0.2);
}

.sc-user-input--button label {
  position: relative;
  height: 24px;
  padding-left: 3px;
  cursor: pointer;
}

.sc-user-input--button label:hover path {
  fill: rgba(86, 88, 103, 1);
}

.sc-user-input--button input {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  z-index: 99999;
  height: 100%;
  opacity: 0;
  cursor: pointer;
  overflow: hidden;
}

.file-container {
  background-color: #f4f7f9;
  border-top-left-radius: 10px;
  padding: 5px 20px;
  color: #565867;
}

.delete-file-message {
  font-style: normal;
  float: right;
  cursor: pointer;
  color: #c8cad0;
}

.delete-file-message:hover {
  color: #5d5e6d;
}

.icon-file-message {
  margin-right: 5px;
}

</style>
