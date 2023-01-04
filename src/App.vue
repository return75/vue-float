<template>
  <div id="app">
    <VueDraggable :left="200" :top="200">
      <div v-for="(i, index) in items" :key="index">
        <h1>alireza</h1>
      </div>
    </VueDraggable>
    <VueDraggable :left="200" :top="200">
      <div v-for="(i, index) in items" :key="index">
        <h1>hamed</h1>
      </div>
    </VueDraggable>

  </div>
</template>

<script>
import VueDraggable from "@/components/VueDraggable.vue";

export default {
  name: 'App',
  data: () => ({
    items: 5
  }),
  components: {
    VueDraggable,
  },
  mounted() {
    // setInterval(() => {
    //   this.items++
    // }, 4000)
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
body {
  margin: 0;
}
</style>
