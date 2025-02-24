<template>
  <div
    class="yk-notification"
    :style="Style"
    @mouseenter="mouseEnter"
    @mouseleave="mouseLeave"
  >
    <div class="notification-container">
      <div class="notification-body">
        <div v-if="props.showIcon" class="iconDiv">
          <IconReminderFill v-if="type === 'primary'" class="icon-primary" />
          <IconWarningFill
            v-else-if="type === 'warning'"
            class="icon-warning"
          />
          <IconCrossFill v-else-if="type === 'error'" class="icon-error" />
          <IconTickFill v-else-if="type === 'success'" class="icon-success" />
          <IconLoadingOutline
            v-else-if="type === 'loading'"
            class="icon-loading"
          />
        </div>
        <div>
          <div class="title-text">{{ title }}</div>
          <div class="content">
            <span class="text">
              <component
                :is="render(message)"
                v-if="!dangerouslyUseHTMLString"
              />
              <p v-else v-html="message" />
            </span>
          </div>
        </div>
        <button
          v-if="props.closable"
          type="button"
          :class="`yk-notification-close-icon`"
          @click="handleClose"
        >
          <IconCloseOutline />
        </button>
      </div>
      <yk-space v-if="props.showFooterBtn" class="notification-footer">
        <yk-button type="secondary" @click="clickCancel">取消</yk-button>
        <yk-button @click="clickOK">确定</yk-button>
      </yk-space>
    </div>
  </div>
</template>
<script setup lang="ts">
import { NotificationProps } from './notification'
import { pauseSetTimeout } from './utils'
import { onMounted, computed, ref } from 'vue'
import { render } from '../../utils'
import '../style'
import { YkButton, YkSpace } from '../../../index'
import IconReminderFill from '../../svg-icon/icon-reminder-fill'
import IconWarningFill from '../../svg-icon/icon-warning-fill'
import IconCrossFill from '../../svg-icon/icon-cross-fill'
import IconTickFill from '../../svg-icon/icon-tick-fill'
import IconLoadingOutline from '../../svg-icon/icon-loading-outline'
import IconCloseOutline from '../../svg-icon/icon-close-outline'

defineOptions({
  name: 'YkNotification',
})
const props = withDefaults(defineProps<NotificationProps>(), {
  title: 'Title',
  message: '',
  type: 'primary',
  duration: 4000,
  closable: true,
  showFooterBtn: false,
  showIcon: true,
  space: 24,
  offsetY: 24,
  offsetX: 24,
  zIndex: 2001,
  dangaurslyUseHtmlString: false,
  handleCancel: () => ({}),
  handleSubmit: () => ({}),
  onClose: () => ({}),
})

const emits = defineEmits(['close', 'lockNotifications', 'unlockNotifications'])

let timer: any = ref()
let remainTime = ref(0)

const Style = computed(() => ({
  marginBottom: `${props.space}px`,
  top: `${props.offsetY}px`,
  right: `${props.offsetX}px`,
  zIndex: props.zIndex,
}))

function mouseLeave() {
  emits('unlockNotifications')
}

function mouseEnter() {
  emits('lockNotifications')
}

function pause() {
  timer.value.pause()
}

function play(firstReaminTime: number) {
  timer.value.play(firstReaminTime)
}

function close() {
  emits('close')
}

function handleClose() {
  close()
}

function clickCancel() {
  props.handleCancel && props.handleCancel()
}

function clickOK() {
  props.handleSubmit && props.handleSubmit()
}

onMounted(() => {
  timer.value = new pauseSetTimeout(() => {
    close()
  }, props.duration)

  if (props.duration > 0) {
    timer.value.play(0, true)
  }
})

defineExpose({
  pause,
  play,
  timer,
  remainTime,
})
</script>
