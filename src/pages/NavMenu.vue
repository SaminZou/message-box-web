<template>
  <el-menu
    class="menu"
    background-color="#f3f7fa"
    text-color="#3e3e3e"
    active-text-color="#0b5ff6"
    :unique-opened="false"
    :default-active="$route.path"
    router
  >
    <el-submenu index="1">
      <template slot="title">
        <i class="el-icon-setting"></i>
        <span>系统配置</span>
      </template>
      <el-menu-item index="/messageconfig/list">消息推送配置</el-menu-item>
      <el-menu-item index="/tool/push">消息推送</el-menu-item>
    </el-submenu>
  </el-menu>
</template>

<script>
export default {
  name: "NavMenu",
  methods: {},
};
</script>

<style scoped>
.el-menu {
  border-right: none;
}

/* .el-menu-item.is-active {
  background-color: #f3f7fa !important;
}

.el-menu-item:hover {
  background-color: #f3f7fa !important;
} */
</style>