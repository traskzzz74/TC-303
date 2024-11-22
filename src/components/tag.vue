<template>
  <span 
    class="seq-tag"
    :class="[
      customClass,
      {
        'is-closable': closable,
        'is-disabled': disabled,
        'seq-tag-select': isSelect
      }
    ]"
  >
    <slot></slot>
    <iconpark-icon 
      v-if="closable"
      name="outline-clear"
      :class="[isSelect ? 'seq-tag-select-close' : 'seq-tag-close']"
      @click.stop="handleClose($event)"
    />
  </span>
</template>

<script>
export default {
  name: 'SeqTag',
  props: {
    closable: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    isSelect: {
      type: Boolean,
      default: false
    },
    customClass: {
      type: [String, Object],
      default: ''
    }
  },
  methods: {
    handleClose(event) {
      if (this.disabled) return
      this.$emit('close', event)
    }
  }
}
</script>

<style lang="scss">
.seq-tag {
  display: inline-flex;
  align-items: center;
  white-space: nowrap;
  gap: var(--seq-space-global-gap-2);

  &.sm {
    padding: 0 var(--seq-space-comp-box-x-sm);
    border-radius: var(--seq-global-radius-33);
    font-size: var(--seq-font-size-body);
    line-height: var(--seq-font-line-height-body);
  }
  &.md {
    padding: 0 var(--seq-space-comp-box-x-md);
    border-radius: var(--seq-global-radius-66);
    font-size: var(--seq-font-size-body);
    line-height: var(--seq-font-line-height-body);
  }
  &.lg {
    padding: 0 var(--seq-space-comp-box-x-lg);
    border-radius: var(--seq-global-radius-66);
    font-size: var(--seq-font-size-body);
    line-height: var(--seq-font-line-height-body);
  }
  &.lgx {
    padding: 0 var(--seq-space-comp-box-x-lgx);
    border-radius: var(--seq-global-radius-66);
    font-size: var(--seq-font-size-lgx);
    line-height: var(--seq-font-line-height-lgx);
  }

  &.primary {
    background: var(--seq-color-bg-brand-1);
    color: var(--seq-color-text-white);
  }
  &.secondary {
    background: var(--seq-color-bg-brand-3);
    color: var(--seq-color-text-brand);
  }

  .seq-tag-close {
    color: currentColor;
    cursor: pointer;
    opacity: 0.7;

    &:hover {
      opacity: 1;
    }
  }

  &.is-disabled {
    cursor: not-allowed;
    opacity: var(--seq-opacity-disabled);

    .seq-tag-close {
      cursor: not-allowed;
    }
  }
}

.seq-tag-select {
  display: inline-flex;
  align-items: center;
  align-self: stretch;
  white-space: nowrap;
  padding: 0 var(--seq-space-comp-box-x-sm);
  border: 1px solid var(--seq-color-stroke-divider-2);
  background: var(--seq-color-bg-index-1);
  font-size: var(--seq-font-size-body);
  line-height: var(--seq-font-line-height-body);
  gap: var(--seq-space-global-gap-2);

  .seq-select-sm & {
    border-radius: calc(var(--seq-global-radius-66) - 0.0625rem);
  }
  .seq-select-md & {
    border-radius: calc(var(--seq-global-radius-66) - 0.0625rem);
  }
  .seq-select-lg & {
    border-radius: calc(var(--seq-global-radius-83) - 0.0625rem);
  }

  .seq-tag-select-close {
    color: var(--seq-color-text-tertiary);
    cursor: pointer;

    &:hover {
      color: var(--seq-color-text-primary);
    }
  }

  &.is-disabled .seq-tag-select-close {
    cursor: not-allowed;
    color: var(--seq-color-text-disabled);
  }
}
</style> 