<template>
  <div class="sidebar">
    <el-menu class="sidebar-el-menu" :default-active="onRoutes" :collapse="collapse" background-color="#324157"
      text-color="#bfcbd9" active-text-color="#20a0ff" unique-opened router>
      <template v-for="item in items">
        <template v-if="isShow(item.index)">
          <template v-if="item.subs">
          <!-- 有次级目录 -->
            <el-submenu :index="item.index" :key="item.index">
              <!-- 一级目录 -->
              <template slot="title">
                <i :class="item.icon"></i><span slot="title">{{ item.title }}</span>
              </template>
              <!-- 次级目录 -->
              <el-menu-item v-for="(subItem,i) in item.subs" :key="i" :index="subItem.index">
                <template v-if="isShow(subItem.index)">
                  {{ subItem.title }}
                </template>
              </el-menu-item>
            </el-submenu>
          </template>
          <template v-else>
          <!-- 无次级目录 -->
            <el-menu-item :index="item.index" :key="item.index">
              <i :class="item.icon"></i><span slot="title">{{ item.title }}</span>
            </el-menu-item>
          </template>
        </template>
      </template>
    </el-menu>
  </div>
</template>

<script>
import bus from '../common/bus'
export default {
  data () {
    return {
      permission: sessionStorage.getItem('permission'),
      collapse: false,
      // 目前采用了固定菜单的形式，从长远看，菜单应该也从数据库获取
      items: [
        {
          icon: 'el-icon-star-on',
          index: 'dashboard',
          title: '平台首页'
        },
        // {
        //   icon: 'el-icon-share',
        //   index: 'users',
        //   title: this.$t('common.mainPage.users'),
        //   limit: 'Admin'
        // },
        {
          icon: 'el-icon-date',
          index: '3',
          title: '课程学习',
          subs: [
            {
              index: 'questLibList',
              title: '题库一览'
            },
            {
              index: 'selfExam',
              title: '自考自测'
            },
            {
              index: 'charts',
              title: '成绩分析'
            }
          ]
        },
        {
          icon: 'el-icon-setting',
          index: 'personal',
          title: '个人设置'
        }
      ]
    }
  },
  computed: {
    onRoutes () {
      return this.$route.path.replace('/', '')
    }
  },
  created () {
    // 通过 Event Bus 进行组件间通信，来折叠侧边栏
    bus.$on('collapse', msg => {
      this.collapse = msg
    })
    // console.log(this.permission.status)
  },
  methods: {
    isShow (page) {
      // switch (this.permission[page]) {
      //   case 'forbidden': return false
      //   case 'execute': return true
      //   case 'read': return true
      //   default: return false
      // }
      return true
    }
  }
}
</script>

<style scoped>
    .sidebar{
        display: block;
        position: absolute;
        left: 0;
        top: 70px;
        bottom:0;
        overflow-y: scroll;
    }
    .sidebar::-webkit-scrollbar{
        width: 0;
    }
    .sidebar-el-menu:not(.el-menu--collapse){
        width: 200px;
    }
    .sidebar > ul {
        height:100%;
    }
</style>
