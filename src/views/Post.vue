<template>
    <el-container>
      <el-header>Header</el-header>
      <el-container>
        <el-aside width="200px">Aside</el-aside>
        <el-container>
          <el-main>Main</el-main>
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
            PostData:[]
        }
    },
    methods:{
        back(){
            this.$router.push('/');
        },
        getPost(id){
            const _this=this;
            this.$axios
            .post("http://localhost:8081/Posts/getPostById/"+id)
            .then((res)=>{
                _this.PostData=res.data;
                console.log(_this.PostData);
            }).catch(function(error){
                console.log(error);
            })
        },
    },
    created(){
        console.log(this.$route.params.PostID);
        this.getPost(this.id);
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
  line-height: 160px;
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
</style>