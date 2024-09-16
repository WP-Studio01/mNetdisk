<template id="main">
  <UserInfo class="main-elements" />
  <Netdisk class="main-elements" />
  <Score class="main-elements" />
</template>

<script>
import UserInfo from './userinfo.vue'
import Netdisk from './netdisk.vue'
import Score from './score.vue'
export default {
  name: 'Main',
  props: {},
  components: {
    UserInfo,
    Netdisk,
    Score
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-elements {
  width: 30%;
  margin: 10px;
  height: 400px;
  background-color: #fafafa;
  float: left;
}
#main {
  margin: 0 auto;
}
</style>
