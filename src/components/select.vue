<template>
  <div 
    class="seq-select" 
    :class="[
      `seq-select-${size}`,
      {
        'is-disabled': disabled,
        'is-selected': selected
      }
    ]"
  >
    <div 
      class="seq-select-trigger"
      tabindex="0"
      :disabled="disabled"
      @click="handleClick"
    >
      <span class="seq-select-label">{{ selectedLabel || placeholder }}</span>
      <iconpark-icon name="fill-caret-down" class="seq-select-arrow"></iconpark-icon>
    </div>

    <div 
      v-show="selected" 
      class="seq-select-dropdown"
    >
      <ul class="seq-select-options">
        <li
          v-for="option in options"
          :key="option.value"
          class="seq-select-option"
          :class="{ 
            'is-selected': modelValue === option.value,
            'blendstroke': currentHoverValue === option.value 
          }"
          @click="handleSelect(option)"
          @mouseenter="handleOptionHover(option)"
          @mouseleave="handleOptionLeave"
        >
          {{ option.label }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SeqSelect',
  props: {
    modelValue: {
      type: [String, Number],
      default: ''
    },
    options: {
      type: Array,
      default: () => []
    },
    placeholder: {
      type: String,
      default: '请选择'
    },
    size: {
      type: String,
      default: 'md',
      validator: (value) => ['sm', 'md', 'lg', 'lgx'].includes(value)
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      selected: false,
      currentHoverValue: null
    }
  },
  computed: {
    selectedLabel() {
      const option = this.options.find(opt => opt.value === this.modelValue)
      return option ? option.label : ''
    }
  },
  methods: {
    handleClick(event) {
      if (this.disabled) return
      this.selected = !this.selected
      this.$emit('click', event)
    },
    handleSelect(option) {
      this.$emit('update:modelValue', option.value)
      this.selected = false
    },
    handleOptionHover(option) {
      this.currentHoverValue = option.value
    },
    handleOptionLeave() {
      this.currentHoverValue = null
    }
  }
}
</script>

<style lang="scss">
.seq-select {
  position: relative;
  display: inline-block;
  width: 100%;
  min-width: 200px;
  font-family: var(--seq-font-family);
  
  &-sm {
    .seq-select-trigger {
      height: var(--seq-size-sm);
      font-size: var(--seq-font-size-sm);
    }
    .seq-select-arrow {
      font-size: 14px;
    }
  }

  &-md {
    .seq-select-trigger {
      height: var(--seq-size-md);
      font-size: var(--seq-font-size-md);
    }
    .seq-select-arrow {
      font-size: 16px;
    }
  }

  &-lg {
    .seq-select-trigger {
      height: var(--seq-size-lg);
      font-size: var(--seq-font-size-lg);
    }
    .seq-select-arrow {
      font-size: 18px;
    }
  }

  &-lgx {
    .seq-select-trigger {
      height: var(--seq-size-lgx);
      font-size: var(--seq-font-size-lg);
    }
    .seq-select-arrow {
      font-size: 20px;
    }
  }

  &-trigger {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    padding: 0 var(--seq-spacing-sm);
    background: var(--seq-color-white);
    border: 1px solid var(--seq-border-color);
    border-radius: var(--seq-border-radius);
    cursor: pointer;
    transition: all var(--seq-transition-duration) var(--seq-transition-timing-function);
    outline: none;

    &:hover {
      border-color: var(--seq-color-primary);
    }

    &:active {
      border-color: var(--seq-color-primary);
    }
  }

  &-label {
    flex: 1;
    color: var(--seq-color-text-regular);
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  &-arrow {
    margin-left: var(--seq-spacing-xs);
    color: var(--seq-color-text-secondary);
    transition: transform var(--seq-transition-duration) var(--seq-transition-timing-function);
  }

  &.is-selected {
    .seq-select-trigger {
      border-color: var(--seq-color-primary);
    }
    .seq-select-arrow {
      transform: rotate(180deg);
    }
  }

  &.is-disabled {
    .seq-select-trigger {
      background-color: var(--seq-color-disabled);
      border-color: var(--seq-border-color);
      color: var(--seq-color-text-placeholder);
      cursor: not-allowed;

      &:hover {
        border-color: var(--seq-border-color);
      }
    }

    .seq-select-arrow {
      color: var(--seq-color-text-placeholder);
      transform: none;
    }
  }

  &-dropdown {
    position: absolute;
    top: calc(100% + var(--seq-spacing-xs));
    left: 0;
    width: 100%;
    background: var(--seq-color-white);
    border: 1px solid var(--seq-border-color);
    border-radius: var(--seq-border-radius);
    box-shadow: var(--seq-shadow-md);
    z-index: var(--seq-z-index-dropdown);
    transform-origin: top;
    animation: dropdownIn var(--seq-transition-duration) var(--seq-transition-timing-function);
  }

  @keyframes dropdownIn {
    from {
      opacity: 0;
      transform: translateY(-20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  &-options {
    margin: 0;
    padding: var(--seq-spacing-xs) 0;
    list-style: none;
    max-height: 250px;
    overflow-y: auto;
  }

  &-option {
    padding: var(--seq-spacing-sm) var(--seq-spacing-md);
    font-size: var(--seq-font-size-md);
    color: var(--seq-color-text-regular);
    cursor: pointer;
    transition: all var(--seq-transition-duration) var(--seq-transition-timing-function);

    &:hover {
      background-color: var(--seq-color-primary-light);
    }

    &.is-selected {
      color: var(--seq-color-primary);
      font-weight: 500;
      background-color: var(--seq-color-primary-light);
    }

    &.blendstroke {
      position: relative;
      &::after {
        content: '';
        position: absolute;
        inset: 0;
        border: 2px solid var(--seq-color-primary);
        border-radius: var(--seq-border-radius-sm);
        opacity: 0.3;
        pointer-events: none;
      }
    }
  }
}
</style> 