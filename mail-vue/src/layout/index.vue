<template>
  <el-container class="layout">
    <el-aside
        class="aside"
        :class="uiStore.asideShow ? 'aside-show' : 'el-aside-hide'">
      <Aside />
    </el-aside>
    <div
        :class="(uiStore.asideShow && isMobile)? 'overlay-show':'overlay-hide'"
        @click="uiStore.asideShow = false"
    ></div>
    <el-container class="main-container">
      <el-main>
        <el-header>
            <Header />
        </el-header>
        <Main />
      </el-main>
    </el-container>
  </el-container>
  <writer ref="writerRef" />
</template>

<script setup>
import Aside from '@/layout/aside/index.vue'
import Header from '@/layout/header/index.vue'
import Main from '@/layout/main/index.vue'
import { ref, onMounted, onBeforeUnmount } from 'vue'
import {useUiStore} from "@/store/ui.js";
import writer from '@/layout/write/index.vue'

const uiStore = useUiStore();
const writerRef = ref({})
const isMobile = ref(window.innerWidth < 1025)
const handleResize = () => {
  isMobile.value = window.innerWidth < 1025
  uiStore.asideShow = window.innerWidth > 1024;
}

onMounted(() => {
  uiStore.writerRef = writerRef

  window.addEventListener('resize', handleResize)
  handleResize()
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})
</script>

<style lang="scss" scoped>
.el-aside-hide {
  position: fixed;
  left: 0;
  height: 100%;
  z-index: 100;
  transform: translateX(-100%);
  transition: all 100ms ease;
}

.aside-show {
  -webkit-box-shadow: var(--aside-right-border);
  box-shadow: var(--aside-right-border);
  transform: translateX(0);
  transition: all 100ms ease;
  z-index: 101;
  @media (max-width: 1025px) {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 101;
    height: 100%;
    background: var(--el-bg-color);
  }
}

.el-aside {
  width: auto;
  transition: all 100ms ease;
}

.layout {
  --el-color-primary: #087f72;
  --el-color-primary-dark-2: #06665c;
  --el-color-primary-light-3: #32998d;
  --el-color-primary-light-9: #e9f5f1;
  --aside-backgound: #f5f8f7;
  --aside-menu-active-background: #e0eee8;
  --aside-right-border: 1px 0 0 var(--el-border-color-lighter);
  --email-hover-background: #f2f8f5;
  --email-scroll-content-color: #737e79;
  font-family: 'Avenir Next', 'PingFang SC', 'Microsoft YaHei', sans-serif;
  height: 100%;
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  overflow: hidden;
}

.layout :deep(.aside .title) {
  background: transparent;
  color: var(--el-text-color-primary);
  justify-content: flex-start;
  gap: 12px;
  height: 54px;
  margin: 12px 16px;
}

.layout :deep(.aside .title > svg) {
  color: var(--el-color-primary);
  flex-shrink: 0;
}

.layout :deep(.aside .el-menu-item) {
  color: var(--el-text-color-regular);
  height: 42px;
  margin: 4px 14px !important;
  padding: 10px 14px !important;
}

.layout :deep(.aside .el-menu-item.choose-item) {
  color: var(--el-color-primary-dark-2);
  font-weight: 600;
}

.layout :deep(.aside .manage-title) {
  color: var(--el-text-color-secondary);
  font-size: 12px;
  margin-top: 28px;
  padding-left: 28px;
}

.layout :deep(.aside .el-menu-item:hover) {
  background: var(--aside-menu-active-background) !important;
}

.layout :deep(.breadcrumb-item) {
  font-weight: 600;
  font-size: 18px;
}

.layout :deep(.header-actions) {
  background: var(--el-fill-color-extra-light);
  border-bottom: 1px solid var(--el-border-color-lighter);
}

.layout :deep(.email-row) {
  border-bottom: 1px solid var(--el-border-color-extra-light);
}

.layout :deep(.email-content) {
  color: var(--email-scroll-content-color);
}

.layout :deep(.email-time), .layout :deep(.phone-time) {
  color: var(--el-text-color-secondary);
  font-size: 12px;
}

:global(.dark) .layout {
  --aside-backgound: #191d1b;
  --aside-menu-active-background: #263d34;
  --el-color-primary: #63c9af;
  --el-color-primary-dark-2: #86dbc5;
  --email-hover-background: #202c26;
  --email-scroll-content-color: #a6b3ac;
}

.main-container {
  min-height: 100%;
  background: var(--el-bg-color);
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}

.el-main {
  padding: 0;
}

.el-header {
  background: var(--el-bg-color);
  border-bottom: solid 1px var(--el-border-color);
  padding: 0 0 0 0;
}

.overlay-show {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.4);
  z-index: 99;
  transition: all 0.3s;
}

.overlay-hide {
  display: flex;
  pointer-events: none;
  opacity: 0;
}
</style>
