<template>
  <div id="app">
    <VueDraggable>
      <div v-for="i in items">
        <h1>alireza</h1>
      </div>
    </VueDraggable>

    <VueDraggable :left="300" :top="100">
      <div v-for="i in items">
        <h1>hamidreza</h1>
      </div>
    </VueDraggable>
  </div>
</template>

<script>
import VueDraggable from "@/components/VueDraggable.vue";


export default {
  name: 'App',
  data: () => ({
    items: 1
  }),
  components: {
    VueDraggable
  },
  mounted() {
    setInterval(() => {
      this.items++
    }, 4000)
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
