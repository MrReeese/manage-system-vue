<template>
  <el-row class="header-wrap">
    <el-col :span="8" class="header">
      <el-col :span="3">
        <el-button
          type="info"
          :icon="isCollapseAside ? 'el-icon-s-unfold' : 'el-icon-s-fold'"
          size="mini"
          ref="collapseMenuBtn"
          @click="collapseMenu"/>
      </el-col>
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <!--        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>-->
        <el-breadcrumb-item>
          <span class="breadcrumb-nav">
            {{currentPosition.label}}
          </span>
        </el-breadcrumb-item>
      </el-breadcrumb>
    </el-col>
    <el-col :span="16" class="header options">
      <common-tags/>
      <el-dropdown @command="handleCommand">
      <span class="el-dropdown-link user-options">
        <el-avatar size="medium" :src="userAvatar"/>
        <span>{{username}}</span>
        <i class="el-icon-arrow-down el-icon--right"/>
      </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item icon="el-icon-user">个人中心</el-dropdown-item>
          <el-dropdown-item icon="el-icon-switch-button" command="logout">注销</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </el-col>
  </el-row>
</template>

<script>
import { mapMutations, mapState } from 'vuex'
import CommonTags from './CommonTags'

export default {
  name: 'CommonHeader',
  components: { CommonTags },
  data () {
    return {
      // 需要使用模块的方式引入img
      username: '用户1',
      userAvatar: require('../assets/imgs/avatar.png')
    }
  },
  computed: {
    ...mapState({
      currentPosition: state => state.headerNav.currentPosition,
      isCollapseAside: state => state.headerNav.isCollapseAside
    })
  },
  methods: {
    ...mapMutations({
      collapseAside: 'collapseAside',
      clearMenu: 'clearMenu',
      clearToken: 'clearToken'
    }),
    collapseMenu () {
      this.collapseAside()
    },
    handleCommand (command) {
      if (command === 'logout') {
        this.logoutHandler()
      }
    },
    logoutHandler () {
      this.clearMenu()
      this.clearToken()
      location.reload() // 刷新浏览器
    }
  }
}
</script>

<style lang="scss" scoped>
  .header-wrap {
    height: 100%;

    .header {
      height: 100%;
      display: flex;
      align-items: center;

      .user-options {
        display: flex;
        align-items: center;
      }

      .el-button {
        width: 35px;
        height: 35px;
        display: flex;
        justify-content: center;
        align-items: center;
        box-sizing: border-box;
        font-size: 1rem;
        background-color: #e7604a;
        border: none;
        border-radius: 0;

        &:active {
          background-color: #693429;
        }
      }

      .el-dropdown-link {
        cursor: pointer;
        color: #dfe4ea;

        .el-avatar {
          margin-right: 5px;
        }
      }

      .el-icon-arrow-down {
        font-size: 12px;
      }

      .demonstration {
        display: block;
        color: #8492a6;
        font-size: 14px;
        margin-bottom: 20px;
      }
    }

    .options {
      justify-content: space-between;
    }
  }
</style>
<style lang="scss">
  /*单独重写UI组件内部样式*/
  .el-breadcrumb__item:last-child .el-breadcrumb__inner {
    color: #dfe4ea;
  }

  .breadcrumb-nav {
    color: #95a5a6;
    font-size: 20px;
  }

  .el-breadcrumb__item:last-child .el-breadcrumb__inner, .el-breadcrumb__item:last-child .el-breadcrumb__inner a, .el-breadcrumb__item:last-child .el-breadcrumb__inner a:hover, .el-breadcrumb__item:last-child .el-breadcrumb__inner:hover {
    color: #95a5a6;
  }
</style>
