<template>
  <button
    :class="[
      'button-base', 
      sizeClass, 
      typeClass, 
      { 
        'disabled': isDisabled,
        'actived': isActived
      }
    ]"
    :disabled="isDisabled"
    @click="handleClick"
    @mouseenter="!isDisabled && (hover = true)"
    @mouseleave="hover = false"
  >
    <iconpark-icon 
      v-if="icon" 
      :name="icon" 
      :size="computedIconSize" 
      :color="computedIconColor" 
    />
    <slot></slot>
  </button>
</template>



<script setup>
import { ref, computed } from 'vue'

const props = defineProps ({
    type: {
      type: String,
      default: 'secondary',
      validator: (value) => ['primary', 'secondary', 'tertiary'].includes(value),
    },
    size: {
      type: String,
      default: 'md',
      validator: (value) => ['sm', 'md', 'lg', 'lgx'].includes(value),
    },
    icon: {
      type: String,
      default: '',
    },
    iconSize: {
      type: Number,
      default: 16,
    },
    isActive: {
      type: Boolean,
      default: false,
    },
    status: {
      type: String,
      default: '',
      validator: (value) => ['', 'isDisabled', 'isActived'].includes(value)
    }
  });

const hover = ref(false)

// 计算类名
const sizeClass = computed(() => `button-${props.size}`)
const typeClass = computed(() => `button-${props.type}`)

// 只保留 disabled 状态计算属性
const isDisabled = computed(() => props.status === 'isDisabled');
const isActived = computed(() => props.status === 'isActived');

// 动态计算图标大小
const computedIconSize = computed(() => {
  switch (props.size) {
    case 'sm':
      return 16;
    case 'md':
      return 16;
    case 'lg':
      return 16;
    case 'lgx':
      return 18;
    default:
      return 16;
  }
})

// 动态计算图标颜色
const computedIconColor = computed(() => {
  if (isDisabled.value) {
    return 'var(--color-text-disabled)';
  } else if (isActived.value || props.isActive) {
    // 激活状态的颜色
    switch (props.type) {
      case 'primary':
      case 'secondary':
        return 'var(--color-text-invert)';
      case 'tertiary':
        return 'var(--color-text-brand)';
      default:
        return 'var(--color-black)';
    }
  } else if (hover.value) {
    // 鼠标悬停时的颜色
    switch (props.type) {
      case 'primary':
        return 'var(--color-text-brand)';
      case 'secondary':
        return 'var(--color-text-invert)';
      case 'tertiary':
        return 'var(--color-text-brand)';
      default:
        return 'var(--color-black)';
    }
  } else {
    // 默认颜色
    switch (props.type) {
      case 'primary':
        return 'var(--color-text-brand)';
      case 'secondary':
        return 'var(--color-text-brand)';
      case 'tertiary':
        return 'var(--color-text-primary)';
      default:
        return 'var(--color-black)';
    }
  }
});

const handleClick = () => {
  console.log('Button clicked');
};

</script>




<style scoped>
@import '/src/styles/tokens.css';

/* formatting model --- positioning schemes / offsets / z-indexes / display / ...  */
/* box model --- sizes / margins / paddings / borders / ...  */
/* typographic --- font / aligns / text styles / ... */
/* visual --- colors / shadows / gradients / ... */

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
  border-radius: var(--radius-400);
  text-align: center;
  transition: all var(--transition-default);

  &.actived,
  &:active {
  /* 当前设计稿设定只有 Primary 样式的按钮有 actived 状态 */
    &.button-primary {
      background: var(--color-button-interaction-bg-brand);
      color: var(--color-text-invert);
    }
  }

  &:hover {
    &.button-primary {
      background: var(--color-interaction-bg-index-5);
    }
    &.button-secondary {
      border: none;
      color: var(--color-text-invert);
      background: var(--color-button-interaction-bg-brand);
    }
    &.button-tertiary {
      color: var(--color-text-brand);
      background: var(--color-interaction-bg-index-2);
    }
  }

  &.disabled {
    cursor: not-allowed;
    pointer-events: none;

    &.button-primary {
      background: var(--color-bg-index-1);
      border-color: var(--color-stroke-divider-1);
      color: var(--color-text-disabled);
    }
    
    &.button-secondary {
      border-color: var(--color-stroke-divider-1);
      color: var(--color-text-disabled);
    }
    
    &.button-tertiary {
      color: var(--color-text-disabled);
    }
  }

}

.button-sm {
  gap: var(--gapping-sm);
  height: var(--size-sm);
  padding: 0 var(--spacing-sm);
  font-size: var(--font-body-size);
  line-height: var(--font-body-line-height);
}

.button-md {
  gap: var(--gapping-md);
  height: var(--size-md);
  padding: 0 var(--spacing-md);
  font-size: var(--font-body-size);
  line-height: var(--font-body-line-height);
}

.button-lg {
  gap: var(--gapping-lg);
  height: var(--size-lg);
  padding: 0 var(--spacing-lg);
  font-size: var(--font-body-size);
  line-height: var(--font-body-line-height);
}

.button-lgx {
  gap: var(--gapping-lgx);
  height: var(--size-lgx);
  padding: 0 var(--spacing-lgx);
  font-size: var(--font-h1-size);
  font-weight: bold;
  line-height: var(--font-h1-line-height);
}

.button-primary {
  position: relative;
  border: 1px solid var(--color-stroke-divider-1);
  color: var(--color-text-brand);
  background: var(--color-interaction-bg-index-4);
}

.button-secondary {
  color: var(--color-text-brand);
  border: 1px solid var(--color-brand);
}

.button-tertiary {
  color: var(--color-text-primary);
}

</style>