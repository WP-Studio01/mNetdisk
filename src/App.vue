<template>
  <h1 id="wc">Welcome</h1>
  <Main />
</template>

<script>
import Main from './components/main.vue'

export default {
  name: 'App',
  components: {
    Main
  }
}
</script>

<style scoped>
#wc{
  text-align: center;
  color: black;
}
</style>
