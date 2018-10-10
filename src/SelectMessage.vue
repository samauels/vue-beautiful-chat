<template>
  <div class="sc-message--select" :style="messageColors">
       <div  :style="{color: messageColors.color}" class="equalHMWrap eqWrap">
    <div v-for="item in data.items" v-bind:key="item.text" class="equalHM eq"><button @click="optionSelected(item.value)" class="sc-message-button">{{item.text}}</button></div>
  </div>
  
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Object,
      required: true
    },
    messageColors: {
      type: Object,
      required: true
    },
    onChooseOption: {
      type: Function,
      required: true
    }
  },
  methods:{
      optionSelected:function(value){
          this.onChooseOption(value)
      }
  }
}
</script>

<style scoped>
.sc-message--select {
  padding: 8px 20px;
  border-radius: 6px;
  font-weight: 300;
  font-size: 12px;
  line-height: 1.2;
  white-space: pre-wrap;
  -webkit-font-smoothing: subpixel-antialiased;
  font-style: italic;
  opacity: .55;
}

.sc-message--meta {
  font-size: xx-small;
  margin-bottom: 0px;
  margin-top: 5px;
  opacity: .5;
  text-align: center;
}
.sc-message-button{
   background-color: orange;
    border: none;
    border-radius: 3px; 
    box-shadow: 0px 0px 2px 0px black;
    padding:5px
}
.sc-message-button:hover{
  
    box-shadow: 0px 0px 6px 0px black;
    cursor:pointer;
}


.equalHMWrap {
	justify-content: space-between;
}
.eqWrap {
	display: flex;
}
.equalHM {
	width: 32%;
}
.eq {
	padding: 10px;
}
</style>
