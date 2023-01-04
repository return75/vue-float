<template>
  <div id="app">
    <VueDraggable :left="200" :top="200" head-title="title" @move="moving">
      <h1>sample component</h1>
      <h1>sample component</h1>
      <h1>sample component</h1>
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
  methods: {
    moving(e) {
      console.log('e', e)
    }
  },
  components: {
    VueDraggable,
  },
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
