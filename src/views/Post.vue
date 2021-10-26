<template>
    <el-container id="Posts">
      <el-header>Header</el-header>
      <el-container>
        <el-aside width="200px">Aside</el-aside>
        <el-container>
          <el-main>
            <div class="content">
              <div class="up">
                <div class="post-title">
                  <h3 style="text-align:left">{{data.PostData.postTopic}}</h3>
                  <el-divider></el-divider>
                </div>
              </div>
              <div class="down">
                <div class="left">
                </div>
                <div class="right">
                  <div class="post-content" @click="printmyData()">
                    {{data.PostData.postContent}}
                  </div>
                  <div class="detail"></div>
                </div>
              </div>
            </div>
            <div class="reply-div">
              <ul id="all-reply">
                <li 
                :class="[curId == item.replyId ? 'active' : '']"
                v-for="item in data.replyData"
                :key="item.replyId"
                >
                  <div class="per-reply">
                    <div class="left"></div>
                    <div class="right">
                      {{item.replyContent}}
                      
                      </div>
                  </div>
                </li>
              </ul>
            </div>
          </el-main>
          <el-footer>Footer</el-footer>
        </el-container>
        <el-aside width="200px">Aside</el-aside>
      </el-container>
    </el-container>
</template>

<script>
import { reactive } from "vue"
import axios from "axios"
import {useRoute} from 'vue-router';

export default{
//     data(){
//         return{
//             id:this.$route.params.PostID,
//             PostData:[],
//             replyData:[],
//         }
//     },
//     methods:{
//         back(){
//             this.$router.push('/');
//         },
//         getPost(id){
//              this.$axios
//               .post("http://localhost:8081/Posts/getPostById/"+id)
//               .then((res) => {
//                 console.log("获取成功");
//                 this.PostData = res.data;
//                 console.log(this.PostData);
//               })
//               .catch(function (error) {
//                 console.log(error);
//               });
//         },
//         getAllReply(id){
//             this.$axios.post("http://localhost:8081/Reply/getAllReplyByPostId/"+id)
//             .then((res) => {
//                 console.log("获取成功");
//                 this.replyData = res.data;
//                 console.log(this.replyData);
//                 console.log(this.replyData);
//               })
//               .catch(function (error) {
//                 console.log(error);
//               });
//         },
//         init(){
//         console.log(this.id1);
//         }
//     },
//     created(){
//       this.$axios.all([this.$axios.post("http://localhost:8081/Reply/getAllReplyByPostId/"+this.id),
//       this.$axios.post("http://localhost:8081/Posts/getPostById/"+this.id)])
//       .then(this.$axios.spread(function(allTask, allCity){
//         console.log('所有请求完成');
//         this.PostData=allTask.data;
//         this.replyData=allCity.data;
//     }))
//     },
//     mounted(){
//       this.init();
//     }
  name:'Posts',
  setup(){
    let data=reactive({
      id:5,
      PostData:[],
      replyData:[],
    })
    getAll();
    // onMounted(()=> {
    //     getAll();
    //     printmyData();
    //   // axios.post("http://localhost:8081/Posts/getPostById/",data.id).then((res)=>{
    //   //   data.PostData=res.data;
    //   // })
    //   // axios.post("http://localhost:8081/Reply/getAllReplyByPostId/",data.id).then((res)=>{
    //   //   data.replyData=res.data;
    //   // })
    //   })
      return{data,printmyData}

      function getAll(){
        const route=useRoute();
        data.id=parseInt(route.params.PostID);
        console.log(data.id);
        axios.all([axios.post("http://localhost:8081/Posts/getPostById/"+data.id),
                   axios.post("http://localhost:8081/Reply/getAllReplyByPostId/"+data.id)])
        .then(axios.spread(function(allTask, allCity){
          console.log('所有请求完成');
          data.PostData=allTask.data;
          data.replyData=allCity.data;
      }))
      }
      function printmyData(){
        console.log(data.id);
        console.log(data.PostData);
        console.log(data.replyData);
      }
  }
}
</script>



<style scoped>
.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: var(--el-text-color-primary);
  text-align: center;
  line-height: 60px;
}

.el-aside {
  background-color: #d3dce6;
  color: var(--el-text-color-primary);
  text-align: center;
  line-height: 200px;
}

.el-main {
  background-color: #e9eef3;
  color: var(--el-text-color-primary);
  text-align: center;
  /* line-height: 160px; */
}

body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
li {
  list-style: none;
}
.down{
  display: flex;
}
.left{
  width: 20%;
}
.right{
  width: 80%;
}
.post-title{
  /* height: 20px; */
  text-align: justify;
}
.post-content{
  text-align: justify;
  height:auto !important;
  height:240px;
  min-height:240px;
  border: solid 1px;
}
</style>