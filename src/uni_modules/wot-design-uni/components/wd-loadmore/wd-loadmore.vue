<template>
  <view :class="['wd-loadmore', customClass]" :style="customStyle" @click="reload">
    <wd-divider v-if="state === 'finished'">{{ finishedText || translate('finished') }}</wd-divider>
    <block v-if="state === 'error'">
      <text class="wd-loadmore__text">{{ errorText || translate('error') }}</text>
      <text class="wd-loadmore__text is-light">{{ translate('retry') }}</text>
      <wd-icon name="refresh" custom-class="wd-loadmore__refresh" />
    </block>
    <block v-if="state === 'loading'">
      <wd-loading custom-class="wd-loadmore__loading" />
      <text class="wd-loadmore__text">{{ loadingText || translate('loading') }}</text>
    </block>
  </view>
</template>

<script lang="ts">
export default {
  name: 'wd-loadmore',
  options: {
    virtualHost: true,
    addGlobalClass: true,
    styleIsolation: 'shared'
  }
}
</script>

<script lang="ts" setup>
import wdDivider from '../wd-divider/wd-divider.vue'
import wdLoading from '../wd-loading/wd-loading.vue'
import wdIcon from '../wd-icon/wd-icon.vue'
import { ref } from 'vue'
import { useTranslate } from '../composables/useTranslate'
import { loadmoreProps, type LoadMoreState } from './types'

const props = defineProps(loadmoreProps)
const emit = defineEmits(['reload'])

const { translate } = useTranslate('loadmore')

const currentState = ref<LoadMoreState | null>(null)

function reload() {
  if (props.state !== 'error') return
  currentState.value = 'loading'
  emit('reload')
}
</script>

<style lang="scss" scoped>
@import './index.scss';
</style>
