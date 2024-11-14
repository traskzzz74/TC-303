<template>
  <div 
    class="seq-select" 
    :class="[
      `seq-select-${size}`,
      {
        'is-disabled': disabled,
        'is-selected': selected,
        'is-multiple': multiple
      }
    ]"
  >
    <div 
      class="seq-select-trigger"
      tabindex="0"
      :disabled="disabled"
      @click="handleClick"
    >
      <div class="seq-select-values">
        <template v-if="multiple && selectedOptions.length">
          <seq-tag
            v-for="option in selectedOptions"
            :key="option.value"
            :size="tagSize"
            closable
            @close="removeOption(option)"
          >
            {{ option.label }}
          </seq-tag>
        </template>
        <span v-else class="seq-select-placeholder">
          {{ selectedLabel || placeholder }}
        </span>
      </div>
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
            'blendstroke': modelValue === option.value 
          }"
          @click="handleSelect(option)"
        >
          {{ option.label }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import SeqTag from './tag.vue'

export default {
  components: {
    SeqTag
  },
  name: 'SeqSelect',
  props: {
    modelValue: {
      type: [String, Number, Array],
      default: () => []
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
      validator: (value) => ['sm', 'md', 'lg'].includes(value)
    },
    disabled: {
      type: Boolean,
      default: false
    },
    multiple: {
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
    },
    selectedOptions() {
      if (!this.multiple) return []
      return this.options.filter(opt => 
        Array.isArray(this.modelValue) && this.modelValue.includes(opt.value)
      )
    },
    tagSize() {
      const sizeMap = {
        'sm': 'sm',
        'md': 'sm',
        'lg': 'md'
      }
      return sizeMap[this.size]
    }
  },
  methods: {
    handleClick(event) {
      if (this.disabled) return
      this.selected = !this.selected
      this.$emit('click', event)
    },
    handleSelect(option) {
      if (this.multiple) {
        const newValue = [...(this.modelValue || [])]
        const index = newValue.indexOf(option.value)
        if (index > -1) {
          newValue.splice(index, 1)
        } else {
          newValue.push(option.value)
        }
        this.$emit('update:modelValue', newValue)
      } else {
        this.$emit('update:modelValue', option.value)
        this.selected = false
      }
    },
    handleOptionHover(option) {
      this.currentHoverValue = option.value
    },
    handleOptionLeave() {
      this.currentHoverValue = null
    },
    removeOption(option) {
      if (this.disabled) return
      const newValue = this.modelValue.filter(v => v !== option.value)
      this.$emit('update:modelValue', newValue)
    }
  }
}
</script>

<style lang="scss">
.seq-select {
  position: relative;
  display: inline-block;
  min-width: 120px;
  
  &-sm {
    .seq-select-trigger {
      gap: var(--seq-gap-comp-sm);
      padding: 0 var(--seq-space-comp-x-sm);
      border-radius: var(--seq-radius-66);
      height: var(--seq-size-comp-sm);
    }
  }

  &-md {
    .seq-select-trigger {
      gap: var(--seq-gap-comp-md);
      padding: 0 var(--seq-space-comp-x-md);
      border-radius: var(--seq-radius-83);
      height: var(--seq-size-comp-md);
    }
  }

  &-lg {
    .seq-select-trigger {
      gap: var(--seq-gap-comp-lg);
      padding: 0 var(--seq-space-comp-x-lg);
      border-radius: var(--seq-radius-100);
      height: var(--seq-size-comp-lg);
    }
  }

  &-trigger {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: var(--seq-color-bg-index-1);
    border: 1px solid var(--seq-color-stroke-divider-2);
    cursor: pointer;

    &:hover {
      border-color: var(--seq-color-brand);
    }
  }

  &-label {
    flex: 1;
    color: var(--seq-color-text-priarmy);
    font-size: var(--seq-font-size-body);
    line-height: var(--seq-font-line-height-body);
    overflow: hidden;
    white-space: nowrap;
  }

  &-suffix {
    color: var(--seq-color-brand);
  }

  &.is-selected {
    .seq-select-trigger {
      border-color: var(--seq-color-brand);
    }

    .seq-select-suffix {
      transform: rotate(180deg);
    }
  }

  &.is-disabled {
    .seq-select-trigger {
      border-color: var(--seq-color-stroke-divider-1);
      color: var(--seq-color-text-tertiary);
      cursor: not-allowed;
    }

    .seq-select-suffix {
      color: var(--seq-color-text-tertiary);
    }
  }

  &-dropdown {
    position: absolute;
    top: calc(100% + var(--seq-space-global-1));
    left: 0;
    width: 100%;
    min-width: inherit;
    background: var(--seq-color-bg-index-1);
    backdrop-filter: var(--seq-effect-blur-md);
    border: 1px solid var(--seq-color-stroke-divider-1);
    border-radius: var(--seq-radius-100);
    z-index: 99;
    transform-origin: top;
    animation: dropdownIn var(--seq-transition-curve);
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
    list-style: none;
  }

  &-option {
    padding: var(--seq-space-comp-y-md) var(--seq-space-comp-x-md);
    border-radius: var(--seq-radius-100);
    font-size: var(--seq-font-size-body);
    line-height: var(--seq-font-line-height-body);
    color: var(--seq-color-text-priamry);
    cursor: pointer;

    &:hover {
      background: var(--seq-color-interactive-bg-index-1);
    }

    &.is-selected {
      background: var(--seq-color-interactive-bg-index-3);
    }
  }

  &-values {
    flex: 1;
    display: flex;
    flex-wrap: wrap;
    gap: var(--seq-spacing-xs);
    min-height: 20px;
    padding: var(--seq-spacing-xs) 0;
  }

  &.is-multiple {
    .seq-select-trigger {
      height: auto;
      min-height: var(--seq-size-md);
    }
  }
}
</style> 