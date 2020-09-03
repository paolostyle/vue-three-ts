<template>
  <MyScene />
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import MyScene from './components/MyScene.vue';

@Component({
  components: {
    MyScene
  }
})
export default class App extends Vue {}
</script>

<style>
body {
  margin: 0;
}
</style>
