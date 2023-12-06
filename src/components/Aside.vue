<template>
  <div>
    <el-menu
      style="height: 100vh"
      default-active="/Home"
      class="el-menu-vertical-demo"
      background-color="#2D3D51"
      text-color="#fff"
      active-text-color="#ffd04b"
      :collapse="collapse"
      :collapse-transition="false"
    >
      <el-menu-item index="/Home">
        <i class="el-icon-s-home"></i>
        <span slot="title">后台首页</span>
      </el-menu-item>

      <el-menu-item index="2">
        <i class="el-icon-s-custom"></i>
        <span slot="title">用户列表</span>
      </el-menu-item>

      <el-submenu index="1">
        <template slot="title">
          <i class="el-icon-s-cooperation"></i>
          <span slot="title">货品管理</span>
        </template>
        <el-menu-item-group>
          <el-menu-item index="1-1">SPU</el-menu-item>
          <el-menu-item index="1-2">SKU</el-menu-item>
        </el-menu-item-group>
      </el-submenu>

      <el-submenu index="3">
        <template slot="title">
          <i class="el-icon-s-marketing"></i>
          <span slot="title">销售统计</span>
        </template>
        <el-menu-item-group>
          <el-menu-item index="3-1">呵呵呵呵</el-menu-item>
          <el-menu-item index="3-2">哈哈哈哈</el-menu-item>
        </el-menu-item-group>
      </el-submenu>
    </el-menu>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isCollapse: false,
    };
  },
  props: ['collapse'],
};
</script>

<style>

</style>