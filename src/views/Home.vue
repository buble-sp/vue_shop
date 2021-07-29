<template>
  <el-container class="container">
    <el-header class="header">
      <div class="logo">
        <img src="../assets/logo.png" />
        <span>电商后台管理系统</span>
      </div>

      <el-button @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <el-aside class="aside" width="200px">
        <el-menu class="menu" background-color="#333744" text-color="#fff" active-text-color="#409bff">
          <el-submenu v-for="item in menuList" :key="item.id" :index="item.id + ''">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>{{ item.authName }}</span>
            </template>
            <el-menu-item v-for="cItem of item.children" :key="cItem.id" :index="cItem.id + ''">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{ cItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main class="main">Main</el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  created () {
    this.getMenuList()
  },
  data () {
    return {
      menuList: []
    }
  },
  methods: {
    logout () {
      window.sessionStorage.removeItem('token')
      this.$router.push('/login')
    },
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
      console.log(this.menuList)
    }
  }
}
</script>
<style lang="less" scoped>
.container {
  height: 100%;
}
.header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 0;
  .logo {
    display: flex;
    align-items: center;
    height: 40px;
    img {
      width: 40px;
      height: 40px;
    }
    span {
      margin-left: 15px;
      color: #fff;
    }
  }
}
.aside {
  background-color: #333744;
  .menu {
    border: 0;
  }
}
.main {
  background-color: #eaedf1;
}
</style>
