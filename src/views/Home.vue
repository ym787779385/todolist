<template>
  <v-container fluid style="padding: 0" >
    <v-layout row>
        <v-flex xs12 text-xs-center style="margin-bottom: 20px">
          <v-card dark color="cyan">
            <v-card-text class="px-0" >{{sum+"个任务，"+complete+"个已完成"}}</v-card-text>
          </v-card>
        </v-flex>
    </v-layout>
    <v-layout row warp :key="index" v-for="(stu,index) in stus" >
      <v-flex xs9 offset-xs1>
         <v-checkbox :label="`${stu.title}`" v-model="stu.ctr"  @change="changestatus(index)"></v-checkbox>
        <p :class="{line : !stu.ctr}"></p>
        <!-- <p>
        <input type="checkbox" value='index' v-model="stu.ctr" @click="changestatus(index)">
        <label :class="{line : stu.ctr}" for="index">{{stu.title}}</label>
        </p> -->
      </v-flex>
      <v-flex xs1 >
         <v-icon @click="isDialogOn(index)">more_vert</v-icon>
      </v-flex>
    </v-layout>
      <v-layout>
        <v-flex>
          <v-card>
           
            <v-card-text style="height: 10px; position: relative">
            <v-btn
              fab 
              @click='insertDalogOn'
              absolute
              top
              dark
              right
              color="pink"
            >
              <v-icon>add</v-icon>
            </v-btn>
          </v-card-text>
        </v-card>
        </v-flex>
      </v-layout>
      <div class="text-xs-center">
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
      </div>
  </v-container>
</template>

<script lang="ts">
import { Component, Vue, Watch } from 'vue-property-decorator';
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
  private stus: any[] = [
    {
      id: 1,
      title: '第一次任务',
      detailconent: '完成就到了快解放',
      ctr: false,
    },
    {
      id: 2,
      title: '第二次任务',
      detailconent: '完成就到了快解放',
      ctr: false,
    },
    {
      id: 3,
      title: '第三次任务',
      detailconent: '完成就到了快解放',
      ctr: false,
    },
  ];

   mounted() {
   this.sum = this.stus.length; 
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
      console.log(insertId);
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
      console.log('aaaaaaaaaaa');
    }else{
      this.complete --;
       this.stus[n].ctr == true;
    }
    
  }
  
}
</script>

<style>
p{
    width:100%;
    margin-left: 3%;
    border-bottom: 1px solid #000;
    margin-top: -37px;
    visibility: none;
  }
  .line{
    visibility: hidden;
  }
</style>

