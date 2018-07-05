<template>
  <v-app>
    <v-toolbar color="indigo lighten-1" dark elevation="2" style="position: fixed; z-index: 900;">
      <v-toolbar-title  style="width: 100%; text-align: center" >{{sum+"个任务 / "+complete+"个已完成"}}</v-toolbar-title>
    </v-toolbar>
    <v-content style="margin-top:80px">
      <router-view @sendMsg="sendGlobalMsg"></router-view>
    </v-content>
  </v-app>
</template>
<script >
  export default {
  data() {
    return {
      sum: 0,
      complete: 0,
    };
  },
  methods: {
    sendGlobalMsg (n) {
      this.sum = n[0];
      this.complete = n[1];
    },
    
  }
};

</script>

<style lang="less">

</style>
