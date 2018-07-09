<template>
    <v-layout fill-height="100%">
      <v-flex xs12>
          <!-- 列表 -->
          <TodoList  @sendMsg="receiveMsg"></TodoList>
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
                    v-model="detailConent"
                  ></v-text-field>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="red darken-1" flat @click.native="delectTodoItem">{{btnText}}</v-btn>
                  <v-btn color="green darken-1" flat @click.native="editTodoItem">保存</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
       
      </v-flex>
    </v-layout>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator"
import TodoList from "@/components/TodoList.vue"
import { watch } from 'fs';

interface ITodoItem {
  id: any,
  title: string,
  detailconent: string,
  ctr: boolean,
}

@Component({
  components: {
    TodoList
  },
})
export default class Home extends Vue {
  private isDialogShow: boolean = false;
  private title: string = '';
  private detailConent: string = '';
  private btnText: string = '删除';
  private listItemId: number = 0;
  private flag: number = 1;
  private todoItemList: ITodoItem[] = [];
  
  private receiveMsg(n: any) {
    this.isDialogShow = true;
    console.log(n);
    this.title = n.title;
    this.detailConent = n.cont;
    this.listItemId = n.index;
  }
  private clear (){
    this.title = '';
    this.detailConent = '';
  }
  private editTodoItem() {
    if(this.flag === 2){
      var list={
      id: 6,
      title: this.title,
      detailConent: this.detailConent,
      ctr: false,
      };
      this.$store.dispatch('insertListAction',list);
      console.log('增加');
    } else {
      let a = {
        index: this.listItemId,
        title: this.title,
        detailConent: this.detailConent,
      }
      this.$store.dispatch('updateListAction',a);
      console.log('修改');
    }
    this.isDialogShow = !this.isDialogShow;
    this.clear();
    this.flag = 1;
  }
  private delectTodoItem() {
    if(this.flag === 1){
      console.log(this.listItemId);
      this.$store.dispatch('deleteListAction',this.listItemId);
      this.isDialogShow = false;
      } else{
      this.clear();
      }
      this.flag = 1;
      this.clear();
  }
  private insertDialog() {
    this.flag =2;
    this.isDialogShow = !this.isDialogShow;
    this.clear();
  }
}
</script>

<style scoped>
  /* .myLabel  {
  text-decoration: line-through;
} */
</style>



