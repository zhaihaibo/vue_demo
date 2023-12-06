<template>
  <div style="height: 100vh;">
    <el-container style="border: 1px solid #eee">
      <el-aside
        :width="asideWidth"
        style="background-color: rgb(238, 241, 246);"
      >
        <Aside :collapse = "collapse"></Aside>
      </el-aside>

      <el-container>
        <el-header style="text-align: right;background-color: white;">
          <Header @doChangeCollapse="doChangeCollapse" :icon = "icon"></Header>
        </el-header>

        <el-main>
          <Main></Main>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import Aside from "./Aside.vue";
import Header from "./Header.vue";
import Main from "./Main.vue";

export default {
  components: {
    Aside,
    Header,
    Main,
  },
  data(){
    return{
        collapse:false,
        asideWidth:'200px',
        icon : 'el-icon-s-fold'
    }
  },
  methods:{
    doChangeCollapse(name){
        console.log(11,name)
        this.collapse = !this.collapse
        if(this.collapse){
            this.asideWidth = '64px'
            this.icon = 'el-icon-s-unfold'
        }else{
            this.asideWidth = '200px'
            this.icon = 'el-icon-s-fold'

        }
    }
  }
};
</script>

<style>
.el-header {
  background-color: #b3c0d1;
  color: #333;
  line-height: 60px;
}

.el-aside {
  color: #333;
}
</style>
