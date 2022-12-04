<template>
  <div class="fixed fullscreen overflow-hidden pointer-events-none" v-if="isRendered">
    <div class="vue-draggable" :style="{left: `${position.left}px`, top: `${position.top}px`, width: `${width}px`}">
      <div class="header" :style="headerStyle"
           @mousedown="startDragging"
           @mouseup="stopDragging"
           @touchmove="handleMove"
      >
        <span @click="toggleShow" class="cursor-pointer">#</span>
        <div>
          <close-icon class="cursor-pointer pointer-events-auto" v-if="!hideCloseBtn" @click="removeDraggable"></close-icon>
          <span class="text-white mr-2">{{headTitle}}</span>
        </div>

<!--        <v-icon color="white" @click="toggleShow">{{isContentVisible ? 'mdi-menu-up' : 'mdi-menu-down'}}</v-icon>-->
      </div>
      <div class="content pointer-events-auto">
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
    contentHeight: null,
    position: {
      left: 0,
      top: 0
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
      this.position.left = e.clientX - 100
      this.position.top = e.clientY - 20
    },
    startDragging() {
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
      let height = document.querySelector('.content').offsetHeight
      console.log('height', height)
      if (height === 0) {
        this.showContent()
      } else {
        this.hideContent()
      }
    },
    showContent() {
      console.log('this.contentHeight', this.contentHeight)
      document.querySelector('.content').style.height = `${this.contentHeight}px`
      this.isContentVisible = true
    },
    hideContent() {
      document.querySelector('.content').style.height = '0px'
      this.isContentVisible = false
    },
    removeDraggable() {
      this.isRendered = false
    },
    handleMove(e) {
      this.position.left = e.touches[0].clientX
      this.position.top = e.touches[0].clientY
    },
  },
  beforeUpdate() {
    let contentDiv = document.querySelector('.content')
    if(contentDiv) {
      this.contentHeight = contentDiv.clientHeight
      if (this.isContentVisible) {
        contentDiv.style.height = 'unset'
      }
    }

  },
  updated() {
    let contentDiv = document.querySelector('.content')
    if(contentDiv) {
      let contentHeight = contentDiv.clientHeight
      this.contentHeight = contentHeight
      if(this.isContentVisible) {
        contentDiv.style.height = `${contentHeight}px`
      }
    }

  },
  mounted() {
    this.setPosition()
  }
}
</script>
<style>
.vue-draggable {
  position: absolute;
  pointer-events: none;
  max-width: 95%;
}
.header {
  pointer-events: auto;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 12px
}
.content {
  overflow: hidden;
  transition: .5s;
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
</style>