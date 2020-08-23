<template>
  <div id="app" class="w-full font-sans">
    <Board v-for="repo in repos" :repo="repo" v-bind:key="repo.id"></Board>
  </div>
</template>

<script>
import Board from './components/Board';

export default {
  name: 'App',
  components: {
    Board
  },
  data() {
    return {
      repos: []
    }
  },
  async created() {
    let res = await window.fetch('https://api.friendsofshopware.com/v2/github/repositories')
    this.repos = await res.json();
  }
}
</script>

<style>
</style>
