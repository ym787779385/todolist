<template>
    <v-layout fill-height="100%">
      <v-flex xs12>
        <v-card> 
          <!-- 列表 -->
          <v-list subheader style="padding: 0">
            <v-list-tile avatar :key="index" v-for="(stu,index) in stus">
              <v-list-tile-action>
                <v-checkbox v-model="stu.ctr" @change="changestatus(index)"></v-checkbox>
              </v-list-tile-action>
              <v-list-tile-content>
                <v-list-tile-title :class="{myLabel: stu.ctr}">{{stu.title}}</v-list-tile-title>
                <v-list-tile-sub-title>{{stu.detailconent}}</v-list-tile-sub-title>
              </v-list-tile-content>
              <v-list-tile-action>
                <v-icon @click="isDialogOn(index)">more_vert</v-icon>
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
                <v-btn color="red darken-1" flat @click.native="isDelected">{{btnText}}</v-btn>
                <v-btn color="green darken-1" flat @click.native="isSaved">保存</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <!-- 浮动按钮 -->
           <v-btn
              fab 
              @click='insertDalogOn'
              fixed
              bottom
              dark
              right
              color="purple lighten-1"
            >
              <v-icon>add</v-icon>
            </v-btn>
        </v-card>
      </v-flex>
    </v-layout>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator"
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src

@Component({
  components: {
    HelloWorld,
  },
})

export default class Home extends Vue {
  private sum: number = 0;
  private complete: number = 0;
  private isDialogShow: boolean = false;
  private stusId: number = -1;
  private title: string = '';
  private btnText: string = '';
  private detailconent: string = '';
  private flag: string = '';
  private stus: any[] = [];

   mounted() {
   this.sum = this.stus.length; 
  }

  @Watch('sum')
  private sendMsg (n:any, o:any) {
    console.log("home 触发事件"+this.complete);
    this.$emit('sendMsg',[this.sum,this.complete]);
  } 

  @Watch('complete')
  private sendMsg2 (n:any, o:any) {
    console.log("home 触发事件"+this.complete);
    this.$emit('sendMsg',[this.sum,this.complete]);
  } 



  private isDialogOn(index: number) {
    this.flag = '修改';
    this.btnText = '删除';
    this.stusId = index;
    this.isDialogShow = true;
    this.title = this.stus[index].title;
    this.detailconent = this.stus[index].detailconent;
  }
  private isSaved() {
    if(this.flag === '修改'){
      this.stus[this.stusId].title = this.title;
      this.stus[this.stusId].detailconent = this.detailconent;
      this.stusId = -1;
    } else{
      let b = this.stus.length - 1;
      let insertId = (b>=0? this.stus[b].id + 1 : 1);
      let a: any = {
        id: insertId,
        title: this.title,
        detailconent: this.detailconent,
        ctr: false,
      };
      this.stus.push(a);
    }
    this.sum = this.stus.length;
    this.isDialogShow = false;
    this.title = '';
    this.detailconent = '';
  }
  private isDelected() {
    if(this.flag === '修改'){
      if(this.stus[this.stusId].ctr == true ){
        this.complete --;
        console.log('sasas')
      }
      this.stus.splice(this.stusId,1);
    } 
    this.sum = this.stus.length;
    this.isDialogShow = false;
    this.title = '';
    this.detailconent = '';
  }
  private insertDalogOn() {
    this.isDialogShow = true;
    this.flag = '增加';
    this.btnText = '取消';
  }
  private changestatus(n: any) {
    if(this.stus[n].ctr == true){
      this.complete ++;
      this.stus[n].ctr == false;
    }else{
      this.complete --;
       this.stus[n].ctr == true;
    }
  }
}
</script>

<style scoped>

</style>

<style>
.myLabel  {
  text-decoration: line-through;
}
</style>



