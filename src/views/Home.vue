<template>
  <v-layout fill-height>
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
    </v-flex>
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
  </v-layout>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
import { State, Action, Getter } from "vuex-class";
import TodoList from "@/components/TodoList.vue";
import { watch } from "fs";
import { mount } from '@vue/test-utils';

interface ITodoItem {
  id: any;
  title: string;
  detailconent: string;
  isChecked: boolean;
}
enum MyEnum {
  Add,
  Delete
}
@Component({
  components: {
    TodoList
  }
})
export default class Home extends Vue {
  @Getter donetodoList !: Array<ITodoItem>;
  private isDialogShow: boolean = false;
  private title: string = "";
  private detailConent: string = "";
  private btnText: string = "删除";
  private listItemId: number = 0;
  private a: MyEnum = MyEnum.Add;

  private receiveMsg(n: any) {
    this.isDialogShow = true;
    console.log(n);
    this.title = n.title;
    this.detailConent = n.cont;
    this.listItemId = n.index;
  }
  private clear() {
    this.title = "";
    this.detailConent = "";
  }
  private editTodoItem() {
    var n = this.donetodoList.length;
    var insertId = (n>0? this.donetodoList[n-1].id + 1 : 0);
    if (this.a === 1) {
      var list = {
        id: insertId,
        title: this.title,
        detailConent: this.detailConent,
        isChecked: false,// isChecked
      };
      this.$store.dispatch("insertListAction", list);
    } else {
      let s = {
        index: this.listItemId,
        title: this.title,
        detailConent: this.detailConent
      };
      this.$store.dispatch("updateListAction", s);
     
    }
    this.isDialogShow = !this.isDialogShow;
    this.clear();
    this.a = MyEnum.Add;
  }
  private delectTodoItem() {
    if (this.a === 0) {
      console.log(this.listItemId);
      this.$store.dispatch("deleteListAction", this.listItemId);
      this.isDialogShow = false;
    } else {
      this.clear();
    }
    this.a = MyEnum.Add;
    this.clear();
  }
  private insertDialog() {
    this.a = MyEnum.Delete;
    this.isDialogShow = !this.isDialogShow;
    this.clear();
  }
}
</script>

<style scoped>
.myLabel  {
  text-decoration: line-through;
}
</style>



