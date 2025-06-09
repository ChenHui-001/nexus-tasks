<script setup>
import { RouterView, RouterLink } from 'vue-router'
import { useAuthStore } from './stores'
import { computed } from 'vue'
import { User, Setting, SwitchButton } from '@element-plus/icons-vue'

const authStore = useAuthStore()
const isAuthenticated = computed(() => authStore.isAuthenticated)
const user = computed(() => authStore.user)

const logout = () => {
  authStore.logout()
  // 添加路由跳转到登录页面
  window.location.href = '/login'
}
</script>

<template>
  <el-container class="app-container">
    <el-header v-if="isAuthenticated">
      <div class="header-container">
        <nav class="nav-section">
          <div class="logo">
            <h1>任务管理系统</h1>
          </div>
          <el-menu mode="horizontal" router class="nav-menu">
            <el-menu-item index="/">首页</el-menu-item>
            <el-menu-item index="/tasks">任务管理</el-menu-item>
            <el-menu-item index="/categories">分类管理</el-menu-item>
          </el-menu>
        </nav>
        <div class="user-info">
          <el-dropdown trigger="click">
            <div class="avatar-wrapper">
              <div class="avatar" v-if="user">{{ user.username.charAt(0).toUpperCase() }}</div>
            </div>
            <template #dropdown>
              <el-dropdown-menu>
                <el-dropdown-item>
                  <span class="dropdown-username">{{ user?.username }}</span>
                </el-dropdown-item>
                <el-dropdown-item divided @click="logout">
                  <el-icon><SwitchButton /></el-icon>
                  退出登录
                </el-dropdown-item>
              </el-dropdown-menu>
            </template>
          </el-dropdown>
        </div>
      </div>
    </el-header>
    <el-main>
      <RouterView />
    </el-main>
  </el-container>
</template>

<style>
/* 全局样式 */
:root {
  --primary-color: #409eff;
  --primary-hover: #66b1ff;
  --primary-light: #409eff;
  --menu-active-bg: rgba(103, 194, 58, 0.15);
  --text-color: #2c3e50;
  --text-light: #606266;
  --border-color: #dcdfe6;
  --background-color: #f5f7fa;
  --header-height: 56px;
  --max-content-width: 1140px;
  --transition-duration: 0.3s;
}

/* 覆盖Element Plus默认菜单样式 */
.el-menu--horizontal > .el-menu-item {
  border-bottom: none !important;
}

.el-menu--horizontal > .el-menu-item.is-active {
  border-bottom: none !important;
}

body {
  margin: 0;
  padding: 0;
  font-family: 'Helvetica Neue', Helvetica, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', '微软雅黑', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: var(--background-color);
  color: var(--text-color);
}

.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* 头部样式 */
.el-header {
  background-color: #fff;
  border-bottom: 1px solid var(--border-color);
  padding: 0;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  height: var(--header-height) !important;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transition: box-shadow 0.3s ease;
}

.el-header:hover {
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
}

.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 100%;
  max-width: var(--max-content-width);
  margin: 0 auto;
  padding: 0 16px;
}

.nav-section {
  display: flex;
  align-items: center;
  flex: 1;
  gap: 16px;
  overflow-x: auto;
  scrollbar-width: none; /* Firefox */
  -webkit-overflow-scrolling: touch;
}

.nav-section::-webkit-scrollbar {
  display: none; /* Chrome, Safari, Edge */
}

.logo {
  flex-shrink: 0;
  margin-right: 0;
}

.logo h1 {
  margin: 0;
  font-size: 1.25rem;
  color: var(--primary-color);
  white-space: nowrap;
  font-weight: 600;
  letter-spacing: 0.5px;
}

.nav-menu {
  flex: 1;
  display: flex;
  min-width: min-content;
  margin: 0;
  padding: 0;
  border-bottom: none !important;
  background-color: transparent !important;
  flex-wrap: nowrap;
  height: 100%;
}

.nav-menu .el-menu-item {
  height: 100%;
  line-height: normal;
  font-size: 14px;
  padding: 0 16px;
  margin: 0 2px;
  transition: all var(--transition-duration) ease;
  white-space: nowrap;
  min-width: fit-content;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  position: relative;
  border-radius: 4px;
}

.nav-menu .el-menu-item {
  position: relative;
  z-index: 1;
}

.nav-menu .el-menu-item::before {
  content: '';
  position: absolute;
  top: 4px;
  bottom: 4px;
  left: 4px;
  right: 4px;
  border-radius: 4px;
  background-color: transparent;
  transition: background-color var(--transition-duration) ease;
  z-index: -1;
}

.nav-menu .el-menu-item:hover::before {
  background-color: var(--menu-active-bg);
}

.nav-menu .el-menu-item.is-active {
  color: var(--primary-color);
  font-weight: 500;
}

.nav-menu .el-menu-item.is-active::before {
  background-color: var(--menu-active-bg);
}

.user-info {
  display: flex;
  align-items: center;
  margin-left: 16px;
}

.avatar-wrapper {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2px;
}

.avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background-color: var(--primary-color);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 500;
  font-size: 14px;
  transition: all var(--transition-duration) ease;
  box-shadow: 0 2px 4px rgba(64, 158, 255, 0.2);
}

.avatar:hover {
  transform: scale(1.05);
  background-color: var(--primary-hover);
  box-shadow: 0 3px 6px rgba(64, 158, 255, 0.3);
}

:deep(.el-dropdown-menu__item) {
  padding: 8px 16px;
  font-size: 14px;
  line-height: 1.5;
}

:deep(.el-dropdown-menu__item--divided) {
  margin-top: 4px;
  border-top-color: var(--border-color);
}

.dropdown-username {
  font-weight: 500;
  color: var(--text-color);
  font-size: 14px;
  padding: 0 4px;
}

.el-dropdown-menu__item {
  display: flex;
  align-items: center;
  gap: 8px;
}

.el-dropdown-menu__item .el-icon {
  font-size: 16px;
  color: var(--primary-color);
}

/* 主内容区域样式 */
.el-main {
  margin-top: var(--header-height);
  padding: 24px;
  flex: 1;
}

.el-main > div {
  max-width: var(--max-content-width);
  margin: 0 auto;
  background-color: #fff;
  border-radius: 8px;
  padding: 24px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
}

/* 响应式布局 */
@media screen and (max-width: 768px) {
  .header-container {
    padding: 0 10px;
  }

  .nav-section {
    flex-wrap: nowrap;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    margin-right: 6px;
    scrollbar-width: none; /* Firefox */
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .nav-section::-webkit-scrollbar {
    display: none; /* Chrome, Safari, Edge */
  }

  .logo {
    margin-right: 0;
    min-width: auto;
    flex-shrink: 0;
  }

  .logo h1 {
    font-size: 1.1rem;
  }

  .nav-menu {
    flex: 1;
    justify-content: flex-start;
    min-width: min-content;
    height: 100%;
  }

  .nav-menu .el-menu-item {
    padding: 0 12px;
    margin: 0 1px;
    font-size: 14px;
    flex-shrink: 0;
    height: 100%;
    line-height: normal;
    display: inline-flex;
    align-items: center;
    border-radius: 4px;
  }

  .nav-menu .el-menu-item.is-active {
    background-color: var(--el-menu-hover-bg-color);
  }

  .nav-menu .el-menu-item:hover {
    background-color: var(--el-menu-hover-bg-color);
  }

  .user-info {
    margin-left: 6px;
  }

  .avatar {
    width: 30px;
    height: 30px;
    font-size: 13px;
  }

  .el-main {
    padding: 10px;
  }

  .el-main > div {
    padding: 14px;
    border-radius: 6px;
  }
}

/* 处理超小屏幕 */
@media screen and (max-width: 480px) {
  :root {
    --header-height: 50px;
  }

  .header-container {
    padding: 0 8px;
  }

  .logo {
    flex-shrink: 0;
  }

  .logo h1 {
    font-size: 1rem;
  }

  .nav-menu {
    margin-left: 8px;
  }

  .nav-menu .el-menu-item {
    padding: 0 10px;
    margin: 0 1px;
    font-size: 13px;
    min-width: max-content;
    height: 100%;
    line-height: normal;
    display: inline-flex;
    align-items: center;
    border-radius: 4px;
  }

  .nav-menu .el-menu-item.is-active {
    background-color: var(--el-menu-hover-bg-color);
  }

  .nav-menu .el-menu-item:hover {
    background-color: var(--el-menu-hover-bg-color);
  }

  .el-main {
    padding: 8px;
  }

  .el-main > div {
    padding: 12px;
    border-radius: 4px;
  }

  .dropdown-username {
    font-size: 13px;
  }

  :deep(.el-dropdown-menu__item) {
    padding: 6px 12px;
    font-size: 13px;
  }
}
</style>
