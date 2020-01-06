<template>
  <!-- Layout整体布局 -->
  <div class="app-wrapper">
    <!-- 导航菜单 -->
    <sidebar class="sidebar-container" />
    <!-- 主体内容 -->
    <el-scrollbar class="app_main_scrollbar" style="height: 100%;">
      <div class="main-container">
        <app-main />
      </div>
    </el-scrollbar>
    <!-- 回到顶部 -->
    <el-backtop target=".app_main_scrollbar .el-scrollbar__wrap" :right="13" />
  </div>
</template>

<script>
import { Sidebar, AppMain } from './components'
import ResizeMixin from './mixin/ResizeHandler'

export default {
  name: 'Layout',
  components: {
    Sidebar,
    AppMain
  },
  mixins: [ResizeMixin],
  computed: {
    sidebar() {
      return this.$store.state.app.sidebar
    },
    device() {
      return this.$store.state.app.device
    },
    fixedHeader() {
      return this.$store.state.settings.fixedHeader
    }
  },
  methods: {
    handleClickOutside() {
      this.$store.dispatch('app/closeSideBar', { withoutAnimation: false })
    }
  }
}
</script>

<style lang="scss" scoped>
  @import "~@/styles/mixin.scss";
  @import "~@/styles/variables.scss";

  .app-wrapper {
    @include clearfix;
    height: 100%;
    width: 100%;
    padding-top: 60px;
  }
</style>
