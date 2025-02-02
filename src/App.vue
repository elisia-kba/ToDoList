<template>
  <TodosList></TodosList>
</template>

<script>
import TodosList from './components/TodosList.vue';

export default {
  name: 'App',
  components: {
    TodosList,
  },
};
</script>

<style>
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
}
@media screen and (max-width: 768px) {
  html, body {
    overflow: auto;
  }
}


body {
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(145deg, #e9d8fd, #00b4d8);
  color: #333;
}

#app {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}
</style>
