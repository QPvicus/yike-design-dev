<template>
  <yk-space
    v-if="type === 'radio'"
    :direction="direction"
    :size="size"
    :type="type"
    :class="bem([type])"
  >
    <slot />
  </yk-space>
  <div v-else :class="bem([type], { solid })">
    <div :class="bem('container', [size])"><slot /></div>
  </div>
</template>

<script setup lang="ts">
import { nextTick, provide, reactive, toRefs } from 'vue'
import { RadioGroupProps, radioGroupEmits } from './radio-group'
import { CHANGE_EVENT, UPDATE_MODEL_EVENT, provideKey } from './constants'
import { YkSpace } from '../../space'
import { createCssScope } from '../../utils'

const emits = defineEmits(radioGroupEmits)

const bem = createCssScope('radio-group')
const props = withDefaults(defineProps<RadioGroupProps>(), {
  direction: 'horizontal',
  type: 'radio',
  size: 'l',
  solid: false,
})
const changeEvent = (value: RadioGroupProps['modelValue']): void => {
  emits(UPDATE_MODEL_EVENT, value)
  nextTick(() => emits(CHANGE_EVENT, value))
}

provide(
  provideKey,
  reactive({
    ...toRefs(props),
    changeEvent,
  }),
)
</script>
