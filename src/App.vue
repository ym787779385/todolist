<template>
  <v-app>
    <v-toolbar color="indigo lighten-1" dark>
      <v-toolbar-title>{{sum+"个任务 / "+complete+"个已完成"}}</v-toolbar-title>
    </v-toolbar>
    <v-content>
      <router-view  v-on:sendMsg="msg"></router-view>
    </v-content>
  </v-app>
</template>
<script>
  export default {
  data() {
    return {
      sum: 0,
      complete: 0,
    };
  },
  methods: {
    msg (n) {
      console.log(1);
      this.sum = n[0];
      this.complete = n[1];
    },
    
  }
};
</script>

<style lang="less">
#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
