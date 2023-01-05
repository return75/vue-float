<template>
    <div v-if="isRendered" class="vue-draggable" :style="{left: `${position.left}px`, top: `${position.top}px`, width: `${width}px`}">
      <div class="header" :style="headStyle"
           @mousedown="startDragging"
           @mouseup="stopDragging"
           @touchmove="handleMove"
      >
        <span @click="toggleShow" class="cursor-pointer caret-down" :class="{'rotate-up': !isContentVisible}"></span>
        <div>
          <span class="text-white mr-2 head-title">{{headTitle}}</span>
          <close-icon class="close-icon" v-if="!hideCloseBtn" @click="removeDraggable"></close-icon>
        </div>
      </div>
      <div class="content" ref="content">
          <slot></slot>
      </div>
    </div>
</template>

<script>
import CloseIcon from './CloseIcon'

export default {
  name: "VueDraggable",
  components: {CloseIcon},
  emits: ['startDrag', 'stopDrag'],
  data: () => ({
    contentHeight: '0',
    headStyle: {
      backgroundColor: '#015bbe',
      color: 'white',
      borderTopLeftRadius: '5px',
      borderTopRightRadius: '5px',
      cursor: 'move',
    },
    position: {
      left: 0,
      top: 0,
      initialLeft: 0,
      initialTop: 0,
    },
    isRendered: true,
    isContentVisible: true,
  }),
  props: {
    width: {
      type: [Number, String],
    },
    headTitle: {
      type: String,
      default: ''
    },
    headIcon: {
      type: String,
      default: ''
    },
    headerStyle: {
      type: Object,
      default: () =>  {}
    },
    left: {
      type: Number,
      default: 0
    },
    top: {
      type: Number,
      default: 0
    },
    hideCloseBtn: {
      type: Boolean,
      default: false
    },
  },
  methods: {
    mouseMoveEvent(e) {
      this.emitMoving(e)
      this.position.left = e.clientX - this.position.initialLeft
      this.position.top = e.clientY - this.position.initialTop
    },
    startDragging(e) {
      this.pauseEvent(e)
      this.position.initialLeft = e.layerX
      this.position.initialTop = e.layerY
      this.emitStartDragging(e)
      document.addEventListener('mousemove', this.mouseMoveEvent, false);
      document.addEventListener('touchmove', this.mouseMoveEvent, false);
    },
    pauseEvent(e){
        if(e.stopPropagation) e.stopPropagation();
        if(e.preventDefault) e.preventDefault();
        e.cancelBubble = true;
        e.returnValue = false;
        return false;
    },
    stopDragging(e) {
      this.emitStopDragging(e)
      document.removeEventListener('mousemove', this.mouseMoveEvent, false);
      document.removeEventListener('touchmove', this.mouseMoveEvent, false);
    },
    setPosition() {
      this.position.left = this.left
      this.position.top = this.top
    },
    toggleShow() {
      if(!this.isContentVisible) {
        this.showContent()
      } else {
        this.hideContent()
      }
    },
    showContent() {
      this.$refs.content.style.height = `${this.contentHeight}px`
      this.isContentVisible = true
    },
    hideContent() {
      this.$refs.content.style.height = '0px'
      this.isContentVisible = false
    },
    removeDraggable() {
      this.isRendered = false
    },
    handleMove(e) {
      this.position.left = e.touches[0].clientX
      this.position.top = e.touches[0].clientY
    },
    setContentHeight() {
      this.contentHeight =  this.$refs.content.clientHeight
      this.$refs.content.style.height = `${this.contentHeight}px`
    },
    mergeHeaderStyle() {
      this.headStyle = Object.assign({}, this.headStyle, this.headerStyle);
    },
    emitStartDragging(e) {
      this.$emit('start-drag', e)
    },
    emitStopDragging(e) {
      this.$emit('stop-drag', e)
    },
    emitMoving(e) {
      this.$emit('move', e)
    }
  },
  beforeUpdate() {
    let contentDiv = this.$refs.content
    if(contentDiv) {
      this.contentHeight = contentDiv.clientHeight
      if (this.isContentVisible) {
        this.$refs.content.style.height = 'unset'
      }
    }
  },
  updated() {
   setTimeout(() => {
     let contentDiv = this.$refs.content
     if(contentDiv) {
       this.contentHeight = contentDiv.clientHeight
       if(this.isContentVisible) {
         this.$refs.content.style.height = `${this.contentHeight}px`
       }
     }
   }, 300)
  },
  mounted() {
    this.setPosition()
    this.setContentHeight()
    this.mergeHeaderStyle()
  }
}
</script>
<style scoped>
.vue-draggable {
  height: auto;
  position: fixed;
  box-sizing: border-box;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px
}
.content {
  overflow: hidden;
  transition: .5s;
  background: #ededed;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
}
.fullscreen {
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
}
.overflow-hidden {
  overflow: hidden;
}
.cursor-default {
  cursor: default;
}
.cursor-pointer {
  cursor: pointer;
}
.caret-down {
  width: 0;
  height: 0;
  display: inline-block;
  border: 6px solid transparent;
  border-top-color: #ffffff;
  transition: .3s;
}
.rotate-up {
  transform: rotateZ(180deg);
  transform-origin: 50% 3px;
}
.close-icon {
  cursor: pointer;
}
.head-title {
  margin-right: 1.5rem;
  margin-left: 1.5rem;
}
</style>