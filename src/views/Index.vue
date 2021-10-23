<template>
  <el-container>
    <el-header>
      <el-row :gutter="10" class="grid-content">
        <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"
          ><div class="grid-content bg-purple">论坛BBS</div></el-col
        >
        <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"
          ><div class="grid-content bg-purple-light">hekai</div></el-col
        >
        <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"
          ><div class="grid-content bg-purple">hekai</div></el-col
        >
        <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"
          ><div class="grid-content bg-purple-light">hekai</div></el-col
        >
      </el-row>
    </el-header>
    <el-container>
      <el-aside width="200px">
        <div class="person-information" :style="{ boxShadow: `var(--el-box-shadow-light)` }">
          <div class="avatar-area">
            <div class="avatar">
              <el-avatar icon="el-icon-user-solid" :size="80"></el-avatar>
            </div>
            
          </div>
          <div class="account-information-area"></div>
          <div class="other-information-area"></div>
        </div>
      </el-aside>
      <el-container>
        <el-main>
          <div class="contentdiv">
            <el-page-header icon="el-icon-arrow-left" content="帖子列表" @back="goBack"/>
            <ul id="content">
              <li
                :class="[curId == item.postID ? 'active' : '']"
                v-for="item in tableData"
                :key="item.postID"
                @click="select(item)"
              >
                <div
                  class="demo-shadow"
                  :style="{ boxShadow: `var(--el-box-shadow-light)` }"
                >
                <br />
                  <div class="post-header horizontal">
                    <div class="post-reply-number">
                      <el-tooltip
                        class="item"
                        effect="light"
                        content="回复"
                        placement="top-start"
                      >
                        <el-tag>{{ item.replyNumber }}</el-tag>
                      </el-tooltip>
                    </div>
                    <div class="post-title">
                        <h3 @click="OpenPost(item)" class="title">{{ item.postTopic }}</h3>
                    </div>
                    <div class="post-author">
                      <span><img src="../img/author-icon.png" alt="" style=""></span>
                      <span>{{ item.postUserName }}</span>
                    </div>
                  </div>
                  <br /><br />
                  <div class="post-body horizontal">
                    <div class="post">
                      {{ item.postContent }}
                    </div>
                    <div class="post-information">
                      <div class="post-update-time">
                        {{ item.updateTime }}
                      </div>
                    </div>
                  </div>
                  <br />
                </div>
               <br> 
              </li>
              <div class="demo-pagination-block">
                <span class="demonstration"></span>
                <el-pagination
                  v-model:currentPage="currentPage2"
                  :page-sizes="[100, 200, 300, 400]"
                  :page-size="100"
                  layout="sizes, prev, pager, next"
                  :total="1000"
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                >
                </el-pagination>
              </div>
            </ul>
            <el-backtop />
          </div>
        </el-main>
        <el-footer>
          @2021 贺凯
        </el-footer>
      </el-container>
      <el-aside width="200px">Aside</el-aside>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        // {
        //   date: '2016-05-03',
        //   name: 'Tom',
        //   address: 'No. 189, Grove St, Los Angeles',
        // },
      ],
    };
  },
  methods: {
    getUsers() {
      const _this = this;
      _this.$axios
        .post("http://localhost:8081/Posts/getPostsPreview")
        .then((res) => {
          console.log("获取成功");
          _this.tableData = res.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    select(item) {
      this.curId = item.postID;
      console.log(item.postID);
    },
    OpenPost(item){
      const id=item.postID;
      console.log(item.postID);
      this.$router.push({path:`/Post/${item.postID}`});
    }
  },
  created() {
    this.getUsers();
  },
};
</script>

<style scoped>
.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: var(--el-text-color-primary);
  text-align: center;
  /* line-height: 60px; */
}

.el-aside {
  background-color: #d3dce6;
  color: var(--el-text-color-primary);
  text-align: center;
  /* line-height: 200px; */
}

.el-main {
  /* background-color: #e9eef3; */
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

.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  margin-top: 5px;
  border-radius: 4px;
  min-height: 36px;
}

.person-information{
  width: 80%;
  height: 20%;
  text-align: center;
  background-color: #606266;
  margin-top: 30%;
  margin-left: 10%;
  border-radius: 10px;
}
.avatar-area{
  height: 40%;
  display:flex;
  align-items:center;
  justify-content:center;
}
.avatar{
  height: 60%;
  display:flex;
  align-items:center;
  justify-content:center;
}
.account-information-area{
  height: 40%;
}
.other-information-area{
  height: 20%;
}
li {
  list-style: none;
}
.horizontal {
  display: flex;
  text-align: center;
}
.post-reply-number {
  margin-top: 15px;
  text-align: center;
  width: 10%;
}
.post-title {
  text-align: left;
  width: 70%;
}
.title:hover{
  cursor: pointer;
  color:#409EFF;
}
.post-author {
  color: #99a9bf;
  width: 20%;
  margin-top: 10px;
  text-align: center;
}
.post {
  text-align: left;
  width: 80%;
  height: 100%;
}
.post-information {
  text-align: center;
  width: 20%;
  color: #99a9bf;
}
.demo-shadow {
  height: 100%;
  width: 100%;
  border: 1px solid var(--el-border-shadow-base);
}
.demo-shadow:hover{
  background-color: #ccd8e6;
}

.demo-pagination-block + .demo-pagination-block {
  margin-top: 10px;
}
</style>