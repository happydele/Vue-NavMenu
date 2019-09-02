<template>
  <div>
    <!-- 顶部水平导航布局 -->
    <logo :collapse="isCollapse" />
    <!-- 导航菜单 -->
    <el-menu
      ref="elmenu"
      :default-active="activeMenu"
      :background-color="variables.menuBg"
      :text-color="variables.menuText"
      :active-text-color="variables.menuActiveText"
      menu-trigger="click"
      unique-opened
      mode="horizontal"
      @close="handleClose"
      @select="handleSelect"
    >
      <sidebar-item v-for="route in permission_routes" :key="route.path" :item="route" :base-path="route.path" />
    </el-menu>
    <!-- 个人信息 -->
    <Navbar />
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Logo from './Logo'
import SidebarItem from './SidebarItem'
import variables from '@/styles/variables.scss'
import Navbar from '../Navbar'

export default {
  components: {
    SidebarItem,
    Logo,
    Navbar
  },
  computed: {
    ...mapGetters([
      'permission_routes',
      'sidebar'
    ]),
    activeMenu() {
      const route = this.$route
      const { meta, path } = route
      // 如果设置路径，侧栏将突出显示您设置的路径
      if (meta.activeMenu) {
        return meta.activeMenu
      }
      return path
    },
    showLogo() {
      return this.$store.state.settings.sidebarLogo
    },
    variables() {
      return variables
    },
    isCollapse() {
      return !this.sidebar.opened
    }
  },
  watch: {
    '$route.path': function(newVal, oldVal) {
      const menu = JSON.parse(sessionStorage.getItem('currentMenu'))
      if (menu) {
        const menuIndex = JSON.parse(sessionStorage.getItem('currentMenu'))[0]
        if (newVal.search(menuIndex) === 0) {
          // 包含
          this.openMenu()
        } else {
          // 不包含
          this.$refs['elmenu'].close(menuIndex)
        }
      }
    }
  },
  mounted() {
    this.openMenu()
  },
  methods: {
    handleClose(key, keyPath) {
      this.openMenu()
    },
    handleSelect(key, keyPath) {
      if (keyPath.length > 1) {
        const menuIndex = [keyPath[0]]
        sessionStorage.setItem('currentMenu', JSON.stringify(menuIndex))
      } else {
        sessionStorage.removeItem('currentMenu')
      }
      // 刷新-重定向到原页面
      const { fullPath } = this.$route
      this.$nextTick(() => {
        this.$router.replace({
          path: '/redirect' + fullPath
        })
      })
    },
    openMenu() {
      const menuIndex = JSON.parse(sessionStorage.getItem('currentMenu'))
      if (menuIndex) {
        this.$refs['elmenu'].open(menuIndex) // 打开二级菜单
      }
    }
  }
}
</script>
