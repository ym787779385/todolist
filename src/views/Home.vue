<template>
  <div class='conter'>
    <v-container>
      <v-layout row>
        <v-flex xs12 text-xs-center>
          <v-card dark color="cyan">
            <v-card-text class="px-0" >{{succnumber+"个任务，"+defnumber+"个已完成"}}</v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
      <v-layout wrap v-for="stu in stus" :key="stu.id" color="deep-purple lighten-5">
        <v-flex xs10>
          <v-checkbox v-model="stu.ctr" :label="`${stu.title}`" @click="changestatus(stu.ctr)"></v-checkbox>
          <p :class="{line : stu.ctr}"></p>
        </v-flex>
        <v-flex xs2>
          <v-dialog  v-model="dialog" persistent max-width="290">
            <v-btn  slot="activator"  dark depressed @click=" updatedText(stu.id)">
              <v-icon>+</v-icon>
            </v-btn>
            <v-card>
              <v-card-title  class="headline">编辑任务</v-card-title>
              <v-card-text>
                <textarea name="" id="" cols="34" rows="10" v-model="textinfo"></textarea>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red darken-1" flat @click.native="deletext">删除</v-btn>
                <v-btn color="green darken-1" flat @click.native="sertext">保存</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-flex>
      </v-layout>
    </v-container>
     <div class="text-xs-center">
         <v-dialog  v-model="dialog2" persistent max-width="290" color="white">
            <v-btn  slot="activator" fab dark left color="purple lighten-4">
              <v-icon>+</v-icon>
            </v-btn>
            <v-card>
              <v-card-title  class="headline">增加任务</v-card-title>
              <v-text-field
                name="input-1-3"
                label="标题"
                single-line
                v-model="titleinfo"
              ></v-text-field>
              <v-text-field
                name="input-7-1"
                label="任务内容"
                multi-line
                 v-model="textinfo2"
              ></v-text-field>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red darken-1" flat @click.native="dialog2=false">取消</v-btn>
                <v-btn color="green darken-1" flat @click.native="insertext">保存</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
    </div>
   
    <!-- 修改对话框 -->
    
    
  
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop, Watch} from 'vue-property-decorator';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src
import { join } from 'path';

@Component({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {
  protected testdata: number = 666;
  private succnumber: number = 0;
  private defnumber: number = 0;
  private alert: boolean = false;
  private stuctr: boolean = false;
  private dialog: boolean = false;
  private dialog2: boolean = false;
  private textinfo: string = '';
  private textinfo2: string = '';
  private titleinfo: string = '';
  private stuid: number = 0;
  private stus: any[] = [
    {
      id: 1,
      title: '第一次任务',
      detailconent: '完成就到了快解放',
      ctr: '',
    },
    {
      id: 2,
      title: '第二次任务',
      detailconent: '完成就到了快解放',
      ctr: '',
    },
    {
      id: 3,
      title: '第三次任务',
      detailconent: '完成就到了快解放',
      ctr: '',
    },
  ];
  mounted() {
   this.succnumber = this.stus.length; 
  }
  @Prop() private msg: string|any;
  @Watch('')
  public onTestDataChange(val: any, oldVal: any) {
    // alert(`当前的值为：${val}`);

  }
  private updatedText(n: number) {
    //用户点击按钮修改任务、删除任务 
    console.log(n);
    this.textinfo = this.stus[n - 1].detailconent;
    console.log(this.textinfo);
    console.log(this.stus);
    this.stuid = n-1;
  }
  private sertext(){
    this.stus[this.stuid].detailconent = this.textinfo;
    this.dialog = false;
  }
  private deletext(){
    if(this.stus[this.stuid].ctr ){
      console.log("已完成"+this.defnumber);
      this.defnumber = this.defnumber - 1;
    }else{
      
    }
    if(this.stus.length == 1){
      this.stus = [];
      this.dialog = false;
    this.succnumber--;
    }else{
      this.stus.splice(this.stuid,1);
    this.dialog = false;
    this.succnumber--;
    }
    
  }
  private insertext(){
    var b = this.stus.length+1;
    var a:any={
      id: b,
      title: this.titleinfo,
      detailconent: this.textinfo2,
      ctr: false,
    }
    this.stus.push(a);
    this.dialog2 = false;
    this.textinfo2 = "";
    this.titleinfo = "";
    this.succnumber++;
  }
  private changestatus(s: any){
    console.log(s)
    if(s == false){
      this.defnumber++;
    }else{
      this.defnumber--;
    }
  }
 
}
</script>
<style scoped>
  /* .conter{
    width: 100%;
    position: relative;
  } */
  .line{
    width: 80%;
    margin-left: 3%;
    border-bottom: 1px solid #000;
    margin-top: -42px;

  }
</style>

