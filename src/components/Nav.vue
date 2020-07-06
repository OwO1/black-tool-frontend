<template>
  <div class="nav">
    <el-menu default-active="1-1" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose" :default-openeds="['1', '2']">
      <el-submenu index="1">
        <template slot="title">
          <i class="el-icon-document"></i>
          <span slot="title">文档类</span>
        </template>
        <el-menu-item index="1-1" @click="goCommands">常用命令</el-menu-item>
        <el-menu-item index="1-2">选项2</el-menu-item>
      </el-submenu>

      <el-submenu index="2" open="false">
        <template slot="title">
          <i class="el-icon-menu"></i>
          <span slot="title">工具类</span>
        </template>
        <el-menu-item index="2-1" @click="goTodos">代办事项列表</el-menu-item>
        <el-menu-item index="2-2" @click="goTransfer">字符转换</el-menu-item>
        <el-menu-item index="2-2" @click="goCompare">Dict/Json对比</el-menu-item>
      </el-submenu>
    </el-menu>
  </div>
</template>

<style>
.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 200px;
  min-height: 400px;
}
</style>

<script>
export default {
  data() {
    return {
      isCollapse: false
    }
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath)
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath)
    },
    goTodos() {
      this.$router.push('/todos/')
    },
    goTransfer() {
      this.$router.push('/transfer/')
    },
    goCompare() {
      this.$router.push('/compare/')
    },
    goCommands() {
      this.$router.push('/commands/')
    }
  }
}
</script>
