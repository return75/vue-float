<template>
  <div class="fixed fullscreen overflow-hidden pointer-events-none" v-if="isRendered">
    <div class="vue-draggable" :style="{left: `${position.left}px`, top: `${position.top}px`, width: `${width}px`}">
      <div class="header" :style="headerStyle"
           @mousedown="startDragging"
           @mouseup="stopDragging"
           @touchmove="handleMove"
      >
        <span @click="toggleShow" class="cursor-pointer caret-down" :class="{'rotate-up': !isContentVisible}"></span>
        <div>
          <close-icon class="cursor-pointer pointer-events-auto" v-if="!hideCloseBtn" @click="removeDraggable"></close-icon>
          <span class="text-white mr-2">{{headTitle}}</span>
        </div>
      </div>
      <div class="content pointer-events-auto" ref="content">
          <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
import CloseIcon from './CloseIcon'

export default {
  name: "VueDraggable",
  components: {CloseIcon},
  data: () => ({
    contentHeight: '0',
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
      type: Number,
      default: 400
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
      default: () =>  ({
        backgroundColor: '#015bbe',
        color: 'white',
        borderTopLeftRadius: '5px',
        borderTopRightRadius: '5px',
        cursor: 'move',
      })
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
      this.position.left = e.clientX - this.position.initialLeft
      this.position.top = e.clientY - this.position.initialTop
    },
    startDragging(e) {
      this.position.initialLeft = e.layerX
      this.position.initialTop = e.layerY
      document.addEventListener('mousemove', this.mouseMoveEvent, false);
      document.addEventListener('touchmove', this.mouseMoveEvent, false);
    },
    stopDragging() {
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
        return;
      }
      if (this.isContentVisible) {
        this.hideContent()
      }

    },
    showContent() {
      console.log('show content')
      this.$refs.content.style.height = `${this.contentHeight}px`
      this.isContentVisible = true
    },
    hideContent() {
      console.log('hide content')
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
      console.log('this.contentHeight', this.contentHeight)
    }
  },
  beforeUpdate() {
    // let contentDiv = this.$refs.contentf
    // if(contentDiv) {
    //   this.contentHeight = contentDiv.clientHeight
    //   if (this.isContentVisible) {
    //     contentDiv.style.height = `${contentDiv.clientHeight}px`
    //   }
    // }
  },
  updated() {
    // let contentDiv = this.$refs.content
    // if(contentDiv) {
    //   let contentHeight = contentDiv.clientHeight
    //   this.contentHeight = contentHeight
    //   if(this.isContentVisible) {
    //     contentDiv.style.height = `${contentHeight}px`
    //   }
    // }
  },
  watch: {
    isContentVisible(val) {
      console.log('isContentVisible', val)
    }
  },
  mounted() {
    this.setPosition()
    this.setContentHeight()
  }
}
</script>
<style>
.fixed {
  position: fixed;
}
.vue-draggable {
  position: absolute;
  pointer-events: none;
}
.header {
  pointer-events: auto;
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
.pointer-events-none {
  pointer-events: none;
}
.pointer-events-auto {
  pointer-events: auto;
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
</style>