<template>
    <v-layout fill-height="100%">
      <v-flex xs12>
        <!-- <v-card>  -->
          <!-- 列表 -->
          <v-list subheader style="padding: 0">
             <v-list-tile avatar :key="index" v-for="(stu,index) in todoItemList">
              <v-list-tile-action>
                <v-checkbox v-model="stu.ctr" @change="changeStatus(index)"></v-checkbox>
              </v-list-tile-action>
              <v-list-tile-content>
                <v-list-tile-title :class="{myLabel: stu.ctr}">{{stu.title}}</v-list-tile-title>
                <v-list-tile-sub-title>{{stu.detailconent}}</v-list-tile-sub-title>
              </v-list-tile-content>
              <v-list-tile-action>
                <v-icon @click="controlDialog(index)">more_vert</v-icon>
              </v-list-tile-action>
            </v-list-tile> 
          </v-list>
          <!-- 对话框 -->
          <v-dialog  v-model="isDialogShow" max-width="390">
            <v-card>
              <v-card-title  class="headline">任务</v-card-title>
                <v-card-text>
                  <v-text-field
                    name="input-1-3"
                    label="标题"
                    single-line
                    v-model="title"
                  ></v-text-field>
                  <v-text-field
                    name="input-7-1"
                    label="任务内容"
                    multi-line
                    v-model="detailconent"
                  ></v-text-field>
                </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red darken-1" flat @click.native="delectTodoItem">{{btnText}}</v-btn>
                <v-btn color="green darken-1" flat @click.native="editTodoItem">保存</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <!-- 浮动按钮 -->
           <v-btn
              fab 
              @click='insertDialog'
              fixed
              bottom
              dark
              right
              color="purple lighten-1"
            >
              <v-icon>add</v-icon>
            </v-btn>
        <!-- </v-card> -->
      </v-flex>
    </v-layout>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator"
import { watch } from 'fs';

interface ITodoItem {
  id: any,
  title: string,
  detailconent: string,
  ctr: boolean,

}

@Component({})
export default class Home extends Vue {
  private sum: number = 0;
  private complete: number = 0;
  private isDialogShow: boolean = false;
  private stusId: number = -1;
  private title: string = '';
  private btnText: string = '';
  private detailconent: string = '';
  private flag: string = '';
  private todoItemList: ITodoItem[] = [];

  mounted() {
    var s = JSON.parse(JSON.stringify(localStorage.getItem('list')));
    s= JSON.parse(s);
    this.todoItemList = s;
    console.log(s);
    // console.log(s);
    this.todoItemList = (s === null ? []:s)
    this.sum = this.todoItemList.length;
    var c = JSON.parse(JSON.stringify(localStorage.getItem('complete')));
    var c= JSON.parse(c);
    this.complete = (c === null ? 0: c);
  }

  @Watch('sum')
  private sendMsg (n:any, o:any) {
    console.log("home 触发事件"+this.complete);
    this.$emit('sendMsg',[this.sum,this.complete]);
    localStorage.setItem('list', JSON.stringify(this.todoItemList));
  } 

  @Watch('complete')
  private sendMsg2 (n:any, o:any) {
    console.log("home 触发事件"+this.complete);
    this.$emit('sendMsg',[this.sum,this.complete]);
    localStorage.setItem('complete',  JSON.stringify(this.complete));
  } 

  @Watch('isDialogShow')
  private clearInput (n:any, o:any) {
    if(n == false){
      this.title = '';
      this.detailconent = '';
      console.log("清空啦");
    }else{
      console.log('显示');
    }
  } 

  private controlDialog(index: number) {
    this.flag = '修改';
    this.btnText = '删除';
    this.stusId = index;
    this.isDialogShow = true;
    this.title = this.todoItemList[index].title;
    this.detailconent = this.todoItemList[index].detailconent;
  }
  private editTodoItem() {
    if(this.flag === '修改'){
      this.todoItemList[this.stusId].title = this.title;
      this.todoItemList[this.stusId].detailconent = this.detailconent;
      this.stusId = -1;
    } else{
      let b = this.todoItemList.length - 1;
      let insertId = (b>=0? this.todoItemList[b].id + 1 : 1);
      let a: any = {
        id: insertId,
        title: this.title,
        detailconent: this.detailconent,
        ctr: false,
      };
      this.todoItemList.push(a);
    }
    this.sum = this.todoItemList.length;
    this.isDialogShow = false;
  }
  private delectTodoItem() {
    if(this.flag === '修改'){
      if(this.todoItemList[this.stusId].ctr == true ){
        this.complete --;
      }
      this.todoItemList.splice(this.stusId,1);
    } 
    this.sum = this.todoItemList.length;
    this.isDialogShow = false;
  }
  private insertDialog() {
    this.isDialogShow = true;
    this.flag = '增加';
    this.btnText = '取消';
  }
  private changeStatus(n: any) {
    if(this.todoItemList[n].ctr == true){
      this.complete ++;
      this.todoItemList[n].ctr == false;
    }else{
      this.complete --;
       this.todoItemList[n].ctr == true;
    }
    localStorage.setItem('list', JSON.stringify(this.todoItemList));
  }
}
</script>

<style scoped>
  .myLabel  {
  text-decoration: line-through;
}
</style>



