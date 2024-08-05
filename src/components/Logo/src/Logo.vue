<script setup lang="ts">
import { ref, watch, computed, onMounted, unref } from 'vue'
import { useAppStore } from '@/store/modules/app'
import { useDesign } from '@/hooks/web/useDesign'

const { getPrefixCls } = useDesign()

const prefixCls = getPrefixCls('logo')

const appStore = useAppStore()

const show = ref(true)

// const title = computed(() => appStore.getTitle)

const layout = computed(() => appStore.getLayout)

const collapse = computed(() => appStore.getCollapse)

const routerLinkClasses = computed(() => {
  return [
    prefixCls,
    layout.value !== 'classic' ? `${prefixCls}__Top` : '',
    'flex !h-[var(--logo-height)] items-center cursor-pointer relative decoration-none overflow-hidden',
    collapse.value ? 'collapsed' : 'expanded'
  ]
})

onMounted(() => {
  if (unref(collapse)) show.value = false
})

watch(
  () => collapse.value,
  (collapse: boolean) => {
    if (unref(layout) === 'topLeft' || unref(layout) === 'cutMenu') {
      show.value = true
      return
    }
    show.value = !collapse
  }
)

watch(
  () => layout.value,
  (layout) => {
    if (layout === 'top' || layout === 'cutMenu') {
      show.value = true
    } else {
      if (unref(collapse)) {
        show.value = false
      } else {
        show.value = true
      }
    }
  }
)
</script>

<template>
  <div>
    <router-link :class="routerLinkClasses" to="/">
      <img
        src="@/assets/imgs/logo.png"
        class="w-[calc(var(--logo-width)-10px)] h-[calc(var(--logo-height)-10px)] ml-15px mt-2px"
      />
      <!-- <div
		v-if="show"
		:class="[
		'ml-10px text-16px font-700',
		{
			'text-[var(--logo-title-text-color)]': layout === 'classic',
			'text-[var(--top-header-text-color)]':
			layout === 'topLeft' || layout === 'top' || layout === 'cutMenu'
		}
		]"
	>
		{{ title }}
	</div> -->
    </router-link>
  </div>
</template>

<style scoped>
.collapsed {
  padding: 0 !important;
  margin: 0 !important;
}

.expanded {
  padding-left: 10px;
  margin-left: 0;
}
</style>
