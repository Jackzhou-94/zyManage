<template>
  <a-layout id="components-layout-demo-responsive" style="min-height:1200px">
    <a-layout-sider
      breakpoint="lg"
      collapsedWidth="0"
      @collapse="onCollapse"
      @breakpoint="onBreakpoint"
    >
      <div class="logo" />
      <a-menu theme="dark" mode="inline" :defaultSelectedKeys="['1']">
        <a-menu-item key="1" @click="add()">
          <router-link to="Home/UserInfoList">
            <a-icon type="user" />
            <span class="nav-text">用户信息</span>
          </router-link>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>
      <a-layout-header :style="{ background: '#fff', padding: 0 }">
        
      </a-layout-header>
      <!-- <div>
          <a-breadcrumb :routes="routes">
            <template slot="itemRender" slot-scope="{route, params, routes, paths}">
              <span v-if="routes.indexOf(route) === routes.length - 1">{{route.breadcrumbName}}</span>
              <router-link v-else :to="`${basePath}/${paths.join('/')}`">{{route.breadcrumbName}}</router-link>
            </template>
          </a-breadcrumb>
          <br />
          {{$route.path}}
        </div> -->
      <a-layout-content :style="{ margin: '24px 16px 0' }">
        <div :style="{ padding: '24px', background: '#fff', minHeight: '360px' }">
          <a-tabs hideAdd v-model="activeKey" type="editable-card" @edit="onEdit">
            <a-tab-pane
              v-for="pane in panes"
              :tab="pane.title"
              :key="pane.key"
              :closable="pane.closable"
            >
              <router-view></router-view>
            </a-tab-pane>
          </a-tabs>
        </div>
      </a-layout-content>
      <a-layout-footer style="textAlign: center">Ant Design ©2018 Created by Ant UED</a-layout-footer>
    </a-layout>
  </a-layout>
</template>
<script>
export default {
  data() {
    const panes = [{ title: "首页", content: "首页", key: "1" }];
    return {
      activeKey: panes[0].key,
      panes,
      newTabIndex: 0,
    /*   basePath: `/${lang}/components/breadcrumb`,
      routes: [
        {
          path: "index",
          breadcrumbName: "首页"
        },
        {
          path: "first",
          breadcrumbName: "一级面包屑"
        },
        {
          path: "second",
          breadcrumbName: "当前页面"
        }
      ] */
    };
  },
  methods: {
    callback(key) {
      console.log(key);
    },
    onEdit(targetKey, action) {
      this[action](targetKey);
    },
    add() {
      const panes = this.panes;
      const activeKey = `newTab${this.newTabIndex++}`;
      panes.push({
        title: this.$route.name,
        content: `Content of new Tab ${activeKey}`,
        key: activeKey
      });
      this.panes = panes;
      this.activeKey = activeKey;
    },
    remove(targetKey) {
      let activeKey = this.activeKey;
      let lastIndex;
      this.panes.forEach((pane, i) => {
        if (pane.key === targetKey) {
          lastIndex = i - 1;
        }
      });
      const panes = this.panes.filter(pane => pane.key !== targetKey);
      if (lastIndex >= 0 && activeKey === targetKey) {
        activeKey = panes[lastIndex].key;
      }
      this.panes = panes;
      this.activeKey = activeKey;
    },
    onCollapse(collapsed, type) {
      console.log(collapsed, type);
    },
    onBreakpoint(broken) {
      console.log(broken);
    }
  }
};
</script>

<style>
#components-layout-demo-responsive .logo {
  height: 32px;
  background: rgba(255, 255, 255, 0.2);
  margin: 16px;
}
</style>