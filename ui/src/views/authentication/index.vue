<template>
  <div class="authentication-setting p-24">
    <h4>{{$t('login.authentication')}}</h4>
    <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
      <template v-for="(item, index) in tabList" :key="index">
        <el-tab-pane :label="item.label" :name="item.name">
          <div class="authentication-setting__main main-calc-height">
            <el-scrollbar>
              <div class="form-container">
                <component :is="item.component" />
              </div>
            </el-scrollbar>
          </div>
        </el-tab-pane>
      </template>
    </el-tabs>
  </div>
</template>
<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import LDAP from './component/LDAP.vue'
import { t } from '@/locales'
const activeName = ref('LDAP')
const tabList = [
  {
    label: t('login.ldap.title'),
    name: 'LDAP',
    component: LDAP
  }
]

// 动态引入组件
const loadComponent = async (componentName: string) => {
  await import(`./component/${componentName}.vue`).then((res) => res.default)
}

const currentComponent = computed(() => loadComponent(activeName.value))

function handleClick() {}

onMounted(() => {})
</script>
<style lang="scss" scoped>
.authentication-setting__main {
  background-color: var(--app-view-bg-color);
  box-sizing: border-box;
  min-width: 700px;
  height: calc(100vh - var(--app-header-height) - var(--app-view-padding) * 2 - 80px);
  box-sizing: border-box;
  .form-container {
    width: 70%;
    margin: 0 auto;
    :deep(.el-checkbox__label) {
      font-weight: 400;
    }
  }
}
</style>
