<template>
    <el-container>
      <el-header>Header</el-header>
      <el-container>
        <el-aside width="200px">Aside</el-aside>
        <el-container>
          <el-main>
            <div class="content">
              <div class="up">
                <div class="post-title">
                  <h3 style="text-align:left">{{PostData.postTopic}}</h3>
                  <el-divider></el-divider>
                </div>
              </div>
              <div class="down">
                <div class="left">
                </div>
                <div class="right">
                  <div class="post-content">
                    {{PostData.postContent}}
                  </div>
                  <div class="detail"></div>
                </div>
              </div>
            </div>
            <div class="reply-div">
              <ul id="all-reply">
                <li
                >
                  <div class="per-reply">
                    <div class="left"></div>
                    <div class="right">
                      <!-- {{item}} -->
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
export default{
    data(){
        return{
            id:this.$route.params.PostID,
            PostData:null,
            replyData:null,
        }
    },
    methods:{
        back(){
            this.$router.push('/');
        },
        getPost(id){
            this.$axios
              .post("http://localhost:8081/Posts/getPostById/"+id)
              .then((res) => {
                console.log("获取成功");
                this.PostData = res.data;
              })
              .catch(function (error) {
                console.log(error);
              });
        },
        getAllReply(id){
            this.$axios.post("http://localhost:8081/Reply/getAllReplyByPostId/"+id)
            .then((res) => {
                console.log("获取成功");
                this.replyData = res.data;
                console.log(this.replyData);
              })
              .catch(function (error) {
                console.log(error);
              });
        },
    },
    mounted() {
      this.getPost(this.id);
      // this.getAllReply(this.id);
    },
    // created(){
    //     this.parallelfunction();
    // }
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