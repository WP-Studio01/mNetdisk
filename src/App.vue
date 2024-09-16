<template>
  <h1 id="wc">欢迎使用角落网盘</h1>
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
