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
      :class="{
        'is-placeholder': multiple && !selectedOptions.length
      }"
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
            :disabled="disabled"
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
      <iconpark-icon name="fill-caret-down" class="seq-select-suffix"></iconpark-icon>
    </div>

    <div 
      v-show="selected" 
      class="seq-select-dropdown"
    >
      <ul v-if="!multiple" class="seq-select-options">
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

      <ul v-else class="seq-select-options">
        <li
          v-for="option in options"
          :key="option.value"
          class="seq-select-multiple-option"
          :class="{ 
            'is-selected': isSelected(option.value)
          }"
          @click="handleSelect(option)"
        >
          {{ option.label }}
          <iconpark-icon 
            v-if="isSelected(option.value)"
            name="outline-check-2"
            class="seq-select-multiple-option-icon"
          />
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
  inject: {
    selectProvider: {
      default: null
    }
  },
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
      if (this.multiple) {
        return ''
      }
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
        'md': 'md',
        'lg': 'lg'
      }
      return sizeMap[this.size]
    }
  },
  mounted() {
    document.addEventListener('click', this.handleOutsideClick)
  },
  beforeDestroy() {
    document.removeEventListener('click', this.handleOutsideClick)
  },
  methods: {
    handleClick(event) {
      if (this.disabled) return
      
      if (!this.selected) {
        if (this.selectProvider) {
          this.selectProvider.setOpenedSelect(this)
        }
        this.selected = true
      } else {
        this.selected = false
        if (this.selectProvider) {
          this.selectProvider.setOpenedSelect(null)
        }
      }
      
      this.$emit('click', event)
      event.stopPropagation()
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
    },
    isSelected(value) {
      return Array.isArray(this.modelValue) && this.modelValue.includes(value)
    },
    handleOutsideClick(event) {
      const isClickInside = this.$el.contains(event.target)
      if (!isClickInside && this.selected) {
        this.selected = false
        if (this.selectProvider) {
          this.selectProvider.setOpenedSelect(null)
        }
      }
    }
  }
}
</script>

<style lang="scss">
.seq-select {
  position: relative;
  display: inline-block;
  min-width: 120px;
  font-size: var(--seq-font-size-body);
  line-height: var(--seq-font-line-height-body);
  
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
      color: var(--seq-color-text-disabled);
      cursor: not-allowed;
    }

    .seq-select-suffix {
      color: var(--seq-color-text-disabled);
    }
  }

  &-dropdown {
    position: absolute;
    top: calc(100% + var(--seq-space-global-2));
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
    color: var(--seq-color-text-priamry);
    cursor: pointer;

    &:hover {
      background: var(--seq-color-interactive-bg-index-1);
    }

    &.is-selected {
      background: var(--seq-color-interactive-bg-index-3);
    }
  }

  &.is-multiple {
    display: inline-flex;
    align-items: center;

    .seq-select-trigger {
      width: 100%;
    }

    &.seq-select-sm .seq-select-trigger {
      padding: var(--seq-space-global-1) 
               var(--seq-space-comp-x-sm) 
               var(--seq-space-global-1) 
               var(--seq-space-global-1);

      &.is-placeholder {
        gap: var(--seq-gap-comp-sm);
        padding: 0 var(--seq-space-comp-x-sm);
      }
    }
    &.seq-select-md .seq-select-trigger {
      padding: var(--seq-space-global-1) 
               var(--seq-space-comp-x-md) 
               var(--seq-space-global-1) 
               var(--seq-space-global-1);

      &.is-placeholder {
        gap: var(--seq-gap-comp-md);
        padding: 0 var(--seq-space-comp-x-md);
      }
    }
    &.seq-select-lg .seq-select-trigger {
      padding: var(--seq-space-global-1) 
               var(--seq-space-comp-x-lg) 
               var(--seq-space-global-1) 
               var(--seq-space-global-1);

      &.is-placeholder {
        gap: var(--seq-gap-comp-lg);
        padding: 0 var(--seq-space-comp-x-lg);
      }
    }

    .seq-select-values {
      display: inherit;
      align-items: inherit;
      align-self: stretch;
      gap: var(--seq-gap-global-1);
    }

  }

  &-multiple-option {
    position: relative;
    padding: var(--seq-space-comp-y-md) var(--seq-space-comp-x-md);
    border-radius: var(--seq-radius-100);
    color: var(--seq-color-text-priamry);
    cursor: pointer;

    &:hover {
      background: var(--seq-color-interactive-bg-index-1);
    }

    &-icon {
      position: absolute;
      right: var(--seq-space-comp-x-sm);
      top: 50%;
      transform: translateY(-50%);
      color: var(--seq-color-brand);
    }
  }

}
</style> 