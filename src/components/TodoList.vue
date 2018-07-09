<template>
  <v-list subheader style="padding: 0">
    <v-list-tile avatar :key="index" v-for="(stu,index) in todoItemList">
      <v-list-tile-action>
        <v-checkbox v-model="stu.ctr"></v-checkbox>
      </v-list-tile-action>
      <v-list-tile-content>
        <v-list-tile-title :class="{myLabel: stu.ctr}">{{stu.title}}</v-list-tile-title>
      <v-list-tile-sub-title>{{stu.detailConent}}</v-list-tile-sub-title>
        </v-list-tile-content>
      <v-list-tile-action>
        <v-icon @click="controlDialog(index)">more_vert</v-icon>
      </v-list-tile-action>
    </v-list-tile> 
  </v-list>
</template>

<script lang="ts">

import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
import { mount } from '@vue/test-utils';
import { State, Action, Getter } from "vuex-class";

interface ITodoItem {
  id: any,
  title: string,
  detailConent: string,
  ctr: boolean,
}

@Component
export default class TodoList extends Vue {

  @Getter donetodoList !: Array<ITodoItem>;

  private isDialogShow: boolean = false;
  private title: string = '';
  private detailConent: string = '';
  private btnText: string = '';
  private todoItemList: ITodoItem[] = [];
  
  @Prop() private msg!: string;
  @Watch('donetodoList')
  private onTodoListsChanged(n:Array<ITodoItem>, o:Array<ITodoItem>) {
      this.todoItemList = n
  }

  mounted() {
    this.todoItemList = this.$store.getters.donetodoList;
    console.log(this.$store.getters. donetodoList); 
  }
  private controlDialog(n: number) {
    this.$emit('sendMsg',{
      title: this.todoItemList[n].title,
      cont: this.todoItemList[n].detailConent,
      index: n,
    });
    console.log('aaa');
}
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">

</style>
