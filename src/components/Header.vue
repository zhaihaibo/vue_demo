<template>
  <div style="display: flex; line-height: 60px">
    <div style="margin-top: 3px">
      <i
        :class="icon"
        style="font-size: 25px; cursor: pointer"
        @click="changeCollapse"
      ></i>
    </div>
    <div style="flex: 1; text-align: center; font-size: 30px">
      <span>欢迎来到后台管理系统</span>
    </div>
    <el-dropdown style="font-size: 18px">
      <span style="cursor: pointer">天王盖地虎</span>
      <i class="el-icon-s-check" style="margin-left: 5px"></i>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item>我的信息</el-dropdown-item>
        <el-dropdown-item>退出登录</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>
</template>

<script>
export default {
  methods: {
    changeCollapse() {
      console.log("aaaa");
      this.$emit("doChangeCollapse", 123);
    },
  },
  props: ["icon"],
};
</script>

<style>
</style>