<template>
  <div id="app">
    <VueDraggable :left="200" :top="500" head-title="lorem ipsum" width="500">
      <p style="padding: 1rem">
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Cum esse ex hic id iure iusto maxime perferendis tempora unde. Amet at dicta eius quae velit? Iste labore laborum officia possimus.
      </p>
    </VueDraggable>
    <div>
      <VueDraggable :left="1000" :top="400" head-title="video" width="600" :header-style="{backgroundColor: '#034879'}">
        <video width="100%" controls style="padding: 1rem">
          <source src="https://www.w3schools.com/html/mov_bbb.mp4" type="video/mp4">
          Your browser does not support the video tag.
        </video>
      </VueDraggable>
    </div>
    <VueDraggable :left="1200" :top="200" head-title="sample text" width="300" :header-style="{backgroundColor: '#02ad79'}">
      <div style="padding: 1rem">
        <h1>this is an example text</h1>
      </div>
    </VueDraggable>
    <VueDraggable v-for="(task, index) in tasks" :left="10 + index * 100" :top="10 + index * 50" :key="index"
                  :head-title="'task-' + (index + 1)" :header-style="{backgroundColor: colors[index]}">
      <div style="padding: 1rem">
        <h1>{{task}}</h1>
      </div>
    </VueDraggable>
    <VueDraggable head-title="audio" width="400" left="600" top="40">
      <div style="padding: 1rem">
        <audio controls>
          <source src="https://samplelib.com/lib/preview/mp3/sample-12s.mp3" type="audio/mpeg">
          Your browser does not support the audio element.
        </audio>
        <audio controls>
          <source src="https://samplelib.com/lib/preview/mp3/sample-15s.mp3" type="audio/mpeg">
          Your browser does not support the audio element.
        </audio>
      </div>
    </VueDraggable>
  </div>
</template>

<script>
import VueDraggable from "./index";

export default {
  name: 'App',
  data: () => ({
    tasks: ['contribution on project 2', 'watching friends', 'cleaning house', 'going to gym', 'running'],
    colors: ['#a60505', '#4b4b4b', '#0109ab', '#e0b405', '#01b059']
  }),
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
