<template>
  <div class="app-container">
    <NotesPage />
  </div>
</template>

<script>
import NotesPage from './components/notes.vue';

export default {
  name: 'App',
  components: {
    NotesPage,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.app-container {
  padding: 20px;
  background-color: #f5f5f5;
  min-height: 100vh;
}
</style>
