<template>
  <div class="hello">
    <v-header/>
    <div>{{msg}}</div>
  </div>
</template>

<script>
import header from "./header/header.vue";

export default {
  name: "HelloWorld",
  components: {
    "v-header": header
  },
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
#app
  .tab-wrapper
    position: fixed
    top: 136px
    left: 0
    right: 0
    bottom: 0
</style>