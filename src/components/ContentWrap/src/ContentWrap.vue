<script setup lang="ts">
import { ElCard, ElTooltip } from 'element-plus'
import { propTypes } from '@/utils/propTypes'
import { useDesign } from '@/hooks/web/useDesign'

const { getPrefixCls } = useDesign()

const prefixCls = getPrefixCls('content-wrap')

defineProps({
  title: propTypes.string.def(''),
  message: propTypes.string.def(''),
  subtitle: propTypes.string.def('')
})
</script>

<template>
  <ElCard :class="[prefixCls]" shadow="never">
    <template v-if="title" #header>
      <div class="flex items-center">
        <span class="text-16px font-700">{{ title }}</span>
        <ElTooltip v-if="message" effect="dark" placement="right">
          <template #content>
            <div class="max-w-200px">{{ message }}</div>
          </template>
          <Icon class="ml-5px" icon="vi-bi:question-circle-fill" :size="14" />
        </ElTooltip>
        <div class="flex pl-20px flex-grow subtitle-right">
          <slot name="header">
            <span class="text-13px font-700">{{ subtitle }}</span>
          </slot>
        </div>
      </div>
    </template>
    <div>
      <slot></slot>
    </div>
  </ElCard>
</template>

<style>
.subtitle-right {
  justify-content: end;
}

.el-table .cell.el-tooltip {
  white-space: wrap;
}
</style>
