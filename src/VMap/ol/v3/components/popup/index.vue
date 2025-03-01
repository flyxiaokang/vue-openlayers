<!--
 * @Description: overlay
 * @Version: 
 * @Author: kangjinrui
 * @Date: 2023-06-16 20:11:12
 * @LastEditors: kangjinrui
 * @LastEditTime: 2024-10-02 10:31:08
-->
<template>
  <div v-show="visible" :id="popupId" :class="getClass">
    <div v-if="showTitle" class="vmap-title">
      <span class="popup-title">{{ title }}</span>
      <span class="popup-title-close" @click="handleClose"></span>
    </div>
    <div :id="contentId" class="vmap-popup-content">
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import {
  ref,
  toRefs,
  onMounted,
  computed,
  nextTick,
  onUnmounted,
} from 'vue'

import { V_THEME } from '@/VMap/global'
import { getClassByTheme } from '@/VMap/public/use/theme'

const props = defineProps({
  theme: {
    type: String,
    default: V_THEME.light,
  },
  visible: {
    type: Boolean,
    default: false,
  },
  showTitle: {
    type: Boolean,
    default: false,
  },
  title: {
    type: String,
    default: '标题',
  },
  popupId: {
    require: true,
    type: String,
    default: '',
  },
  contentHtml: {
    type: String,
    default: '',
  },
})
const { theme, popupId } = toRefs(props)
const emits = defineEmits(['on-close'])

let contentId = ref(popupId.value + '_content')

const handleClose = () => {
  emits('on-close')
}

const getClass = computed(() => {
  return ['vmap-ol-popup', theme.value]
})

onMounted(() => {
  nextTick((e) => {})
})

onUnmounted(() => {})
</script>

<style lang="scss" scoped>
.vmap-ol-popup {
  position: absolute;
  background-color: rgba(255, 255, 255, 0) !important;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.2);
  // border: 1px solid #cccccc;
  bottom: 12px;
  left: -50px;
  min-width: 99px !important;
  border-radius: 8px;
}

.vmap-title {
  width: 100%;
  height: 37px;
  line-height: 35px;
  border-top-left-radius: 6px;
  border-top-right-radius: 6px;
}

.light .vmap-title {
  background-color: #409eff;
}

.dark .vmap-title {
  background-color: rgba(0, 0, 0, 0.5);
  color: black;
}

.vmap-ol-popup:after,
.vmap-ol-popup:before {
  top: 100%;
  border: solid transparent;
  content: ' ';
  height: 0;
  width: 0;
  position: absolute;
  pointer-events: none;
}

.vmap-ol-popup.light:after {
  border-top-color: white;
  border-width: 10px;
  left: 48px;
  margin-left: -10px;
}

.vmap-ol-popup.dark:after {
  border-top-color: rgb(0, 0, 0, 0.5);
  border-width: 10px;
  left: 48px;
  margin-left: -10px;
}

.vmap-ol-popup:before {
  border-top-color: #cccccc;
  border-width: 11px;
  left: 48px;
  margin-left: -11px;
}

.popup-title-close:after {
  content: '✖';
}

.popup-title {
  position: absolute;
  color: white;
  font-weight: bold;
  top: 0px;
  left: 8px;
}

.popup-title-close {
  text-decoration: none;
  position: absolute;
  top: 2px;
  right: 8px;
  color: white;
  font-size: 20px;
  cursor: pointer;
}

.light .vmap-popup-content {
  // background-color: white;
  background: aliceblue;
  color: black;
}

.dark .vmap-popup-content {
  // background-color: rgba(0, 0, 0, 0.5);
  color: white;
}

.vmap-popup-content {
  width: 100%;
  // padding: 3px 3px;
  text-align: center;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  // max-height: 300px;
  // overflow-y: auto;
}
</style>
