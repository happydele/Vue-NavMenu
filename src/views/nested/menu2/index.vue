<template>
  <div style="padding:60px;">
    <el-alert :closable="false" title="menu 2" />
    <el-row style="margin-top: 30px;">
      <el-button round @click="jump1">Dashboard</el-button>
      <el-button type="primary" round @click="jump2">Form</el-button>
      <el-button type="success" round @click="jump3">Table</el-button>
      <el-button type="info" round>信息按钮</el-button>
      <el-button type="warning" round>警告按钮</el-button>
      <el-button type="danger" round>危险按钮</el-button>
    </el-row>
  </div>
</template>

<script>
export default {
  methods: {
    jump1() {
      this.$router.push({ path: '/dashboard' })
    },
    jump2() {
      this.$router.push({ path: '/form/index' })
    },
    jump3() {
      this.$router.push({ path: '/example/table' })
    }
  }
}
</script>
