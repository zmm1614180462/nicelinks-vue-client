<template>
<div class="side-nav">
  <el-menu :default-openeds="['1']" default-active="1" class="el-menu-vertical-demo"
    @open="handleOpen" @close="handleClose" theme="light">
    <el-submenu index="1">
      <template slot="title">
        <i class="sidenav-icon el-icon-menu"></i>{{ $t('niceLinksStr') }}
      </template>
      <el-menu-item v-for="(item, index) in navList"
        :key="item.value" @click="handleClick(item)" :index="item.name">
        {{ $t(item.name) }}
      </el-menu-item>
    </el-submenu>
    <el-menu-item index="2" @click="onActivateInjectDlg">
      <i class="sidenav-icon el-icon-plus"></i>{{ $t('injectLinks') }}
    </el-menu-item>
    <el-menu-item index="3" @click="onThemeClick">
      <icon class="sidenav-icon" name="theme"></icon>{{ $t('themeCollection') }}
    </el-menu-item>
    <el-menu-item index="4" @click="onTagsClick">
      <icon class="sidenav-icon" name="tag"></icon>{{ $t('tagsCollection') }}
    </el-menu-item>
    <el-menu-item index="5" @click="onSwitchLangClick">
      <icon class="sidenav-icon" name="switch-lang"></icon>{{ $t('switchLang') }}
    </el-menu-item>
    <el-menu-item index="6" v-if="isAdminFlag" @click="onManageClick">
      <i class="sidenav-icon el-icon-setting"></i>{{ $t('management') }}
    </el-menu-item>
  </el-menu>
</div>
</template>

<script>
import $config from 'config'

export default {
  name: 'SideNav',
  data () {
    return {
      navList: $config.classify
    }
  },

  components: {
  },

  computed: {
    isAdminFlag () {
      if (this.userInfo) {
        return this.userInfo && this.userInfo.role === 'Admin'
      }
    }
  },

  mounted () {
  },

  methods: {
    onActivateInjectDlg () {
      this.$bus.emit('activate-inject-dlg')
      this.triggerSideNav()
    },

    onThemeClick () {
      this.$router.push('/collections/theme')
      this.triggerSideNav()
    },

    onTagsClick () {
      this.$router.push('/collections/tags')
      this.triggerSideNav()
    },

    onSwitchLangClick () {
      this.$switchLang()
      this.triggerSideNav()
    },

    onManageClick () {
      this.$router.push('/manage')
      this.triggerSideNav()
    },

    handleClick (item) {
      this.$router.push(`/${item.name}`)
      this.$bus.emit('fetch-search', {
        'classify': item.value
      })
      this.triggerSideNav()
    },

    triggerSideNav () {
      this.$bus.$emit('trigger-sidenav')
    },

    handleOpen () {
    },

    handleClose () {
    }
  }
}
</script>

<style media="screen" lang="scss">
@import "./../assets/scss/variables.scss";
  .side-nav{
    text-align: left;
    height: 100%;
    min-height: 100%;
    position: fixed;
    overflow-y: scroll;
    left: 0;
    top: $header-height;
    z-index: 1000;
    width: 210px;
    &::-webkit-scrollbar {
      background: transparent;
      width: 0px;
    }
    &:hover::-webkit-scrollbar {
      background: transparent;
      width: 0px;
    }
    .el-menu-item{
      display: flex;
      align-items: center;
    }
    .sidenav-icon{
      width: 16px;
      height: 16px;
      margin-right: 10px;
      color: #130c0e;
    }
  }
</style>
