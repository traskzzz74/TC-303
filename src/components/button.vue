<template>
  <button
    class="button-base"
    :class="[
      props.type, 
      props.status
    ]"
    :disabled="isDisabled"
    :active="active"
    ref="buttonRef"
  >
    <iconpark-icon 
      v-if="icon"
      :name="icon"
      :size="computedIconSize"
    />
    <slot></slot>
  </button>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const buttonRef = ref(null)

const getSize = () => {
  const classList = buttonRef.value?.classList
  if (classList) {
    if (classList.contains('lgx')) return 'lgx'
    if (classList.contains('lg')) return 'lg'
    if (classList.contains('md')) return 'md'
    if (classList.contains('sm')) return 'sm'
  }
  return 'md'
}

const computedIconSize = computed(() => {
  const size = getSize()
  switch (size) {
    case 'sm': return 16;
    case 'md': return 16;
    case 'lg': return 16;
    case 'lgx': return 20;
    default: return 16;
  }
});

const observer = new MutationObserver(() => {
  computedIconSize.value = getSize()
})

onMounted(() => {
  if (buttonRef.value) {
    observer.observe(buttonRef.value, {
      attributes: true,
      attributeFilter: ['class']
    })
  }
})

const props = defineProps({
  type: {
    type: String,
    required: true,
    validator: (value) => ['primary', 'secondary', 'tertiary'].includes(value),
  },
  icon: {
    type: String,
    default: '',
  }
});

const hover = ref(false)
const buttonClasses = computed(() => {
  const classes = ['button-base'];
  
  classes.push(props.type); 
  classes.push(props.size);
  
  return classes.join(' ');
});

const handleClick = () => {
  console.log('Button clicked');
};

</script>

<style lang="scss">
.button-base {
  position: relative;
  z-index: 1;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  flex-shrink: 0;
  overflow: hidden;
  cursor: pointer;
  border: none;
  border-radius: var(--seq-radius-400);
  text-align: center;
  transition: all var(--seq-transition-default);

  &.is-circle {
    aspect-ratio: 1;
    padding: 0;
    width: auto;
  }

  &.sm {
    gap: var(--seq-gapping-sm);
    height: var(--seq-size-sm);
    padding: 0 var(--seq-spacing-comp-x-sm);
    font-size: var(--seq-font-body-size);
    line-height: var(--seq-font-body-line-height);
  }
  &.md {
    gap: var(--seq-gapping-md);
    height: var(--seq-size-md);
    padding: 0 var(--seq-spacing-comp-x-md);
    font-size: var(--seq-font-body-size);
    line-height: var(--seq-font-body-line-height);
  }
  &.lg {
    gap: var(--seq-gapping-lg);
    height: var(--seq-size-lg);
    padding: 0 var(--seq-spacing-comp-x-lg);
    font-size: var(--seq-font-body-size);
    line-height: var(--seq-font-body-line-height);
  }
  &.lgx {
    gap: var(--seq-gapping-lgx);
    height: var(--seq-size-lgx);
    padding: 0 var(--seq-spacing-comp-x-lgx);
    font-size: var(--seq-font-h3-size);
    font-weight: bold;
    line-height: var(--seq-font-h1-line-height);
  }

  // 设计稿设定当前只有 primary 样式有 active 状态
  &.primary {
    color: var(--seq-color-text-brand);
    background: var(--seq-color-interactive-bg-index-4);

    &:hover {
      background: var(--seq-color-interactive-bg-index-5);
    }

    &:active {
      background: var(--seq-color-button-interactive-bg-brand);
      color: var(--seq-color-text-invert);
    }

  }
  &.secondary {
    border: 1px solid var(--seq-color-brand);
    color: var(--seq-color-text-brand);

    &:hover {
      border: none;
      color: var(--seq-color-text-invert);
      background: var(--seq-color-button-interactive-bg-brand);
    }

  }
  &.tertiary {
      border: none;
      color: var(--seq-color-text-primary);

      &:hover {
        color: var(--seq-color-text-brand);
        background: var(--seq-color-interactive-bg-index-2);
    }

  }

  &:disabled {
    cursor: not-allowed;
    pointer-events: none;

    &.primary {
      border: 1px solid var(--seq-color-stroke-divider-1);
      color: var(--seq-color-text-disabled);
      background: var(--seq-color-bg-index-1);
    }
    &.secondary {
      border: 1px solid var(--seq-color-stroke-divider-1);
      color: var(--seq-color-text-disabled);
    }
    &.tertiary {
      border: none;
      color: var(--seq-color-text-disabled);
    }

  }

}

</style>