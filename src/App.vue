<template>
  <div id="app">
    <div class="intro">
      <h1>Animated Image Grid</h1>
      <h3>with GreenSock, Vue</h3>
    </div>
    <image-grid />
  </div>
</template>

<script>
import ImageGrid from './ImageGrid';

export default {
  name: 'App',
  components: {
    ImageGrid,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
body {
  background: black;
  margin: 0;
  color: #ddd;
  text-align: center;
  font-family: sans-serif;
}
.intro {
  margin: 5rem;
}
</style>
