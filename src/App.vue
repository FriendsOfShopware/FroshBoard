<template>
  <div class="w-full font-sans gap-x-1 flex bg-sky-600 min-h-dvh flex-wrap">
    <Board v-for="repo in repos" :repo="repo" v-bind:key="repo.id"></Board>
  </div>
</template>

<script>
import Board from './components/Board.vue';

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
    let res = await window.fetch('https://api.friendsofshopware.com/v2/github/repositories');
    this.repos = await res.json();
    this.repos = this.repos.sort(function(a, b) {
      const nameA = a.name.toUpperCase();
      const nameB = b.name.toUpperCase();
      if (nameA < nameB) {
        return -1;
      }

      if (nameA > nameB) {
        return 1;
      }

      return 0;
    });
  }
}
</script>

<style>
</style>
