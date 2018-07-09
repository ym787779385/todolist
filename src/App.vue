<template>
  <v-app>
    <v-toolbar color="indigo lighten-1" dark elevation="2" style="position: fixed; z-index: 900;">
      <v-toolbar-title  style="width: 100%; text-align: center" >{{sum+"个任务 / "+complete+"个已完成"}}</v-toolbar-title>
    </v-toolbar>
    <v-content style="margin-top:80px">
      <router-view ></router-view>
    </v-content>
  </v-app>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator"
import { State, Action, Getter } from "vuex-class";
interface ITodoItem {
  id: any,
  title: string,
  detailconent: string,
  ctr: boolean,
}
@Component
export default class App extends Vue  {
  @Getter todoListNumber !: Array<ITodoItem>;
  @Getter donetodoList !: Array<ITodoItem>;
  private sum: number = 0;
  private complete: number = 0;
  @Watch('todoListNumber')
    private onTodoListChanged(n:Array<ITodoItem>, o:Array<ITodoItem>) {
      this.complete = n.length;
      console.log(this.complete);
      this.$emit('sendMsg',[this.sum,this.complete]);
  }
  @Watch('donetodoList')
    private onTodoListsChanged(n:Array<ITodoItem>, o:Array<ITodoItem>) {
      this.sum = n.length;
      this.$emit('sendMsg',[this.sum,this.complete]);
  }
  mounted() {
    this.sum = this.donetodoList.length;
    this.complete = this.todoListNumber.length;
    this.$emit('sendMsg',[this.sum,this.complete]);
  }
}
</script>

<style lang="less">

</style>
