<template>
  <span 
    class="seq-tag"
    :class="[
      `seq-tag-${size}`,
      `seq-tag-${type}`,
      {
        'is-closable': closable,
        'is-disabled': disabled
      }
    ]"
  >
    <slot></slot>
    <iconpark-icon 
      v-if="closable"
      name="outline-clear"
      class="seq-tag-close"
      @click.stop="!disabled && handleClose"
    />
  </span>
</template>

<script>
export default {
  name: 'SeqTag',
  props: {
    type: {
      type: String,
      default: 'default',
      validator: (value) => ['default', 'primary', 'success', 'warning', 'danger'].includes(value)
    },
    size: {
      type: String,
      default: 'md',
      validator: (value) => ['sm', 'md', 'lg'].includes(value)
    },
    closable: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    handleClose(event) {
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

  &-sm, &-md, &-lg {
    align-self: stretch;
    gap: var(--seq-gap-global-2);
    font-size: var(--seq-font-size-body);
    line-height: var(--seq-font-line-height-body);
  }

  &-sm {
    padding: 0 var(--seq-space-comp-x-sm);
    border-radius: var(--seq-radius-33);
  }
  &-md {
    padding: 0 var(--seq-space-comp-x-md);
    border-radius: var(--seq-radius-66);
  }
  &-lg {
    padding: 0 var(--seq-space-comp-x-lg);
    border-radius: var(--seq-radius-66);
  }
  &-lgx {
    align-self: stretch;
    padding: 0 var(--seq-space-comp-x-lgx);
    font-size: var(--seq-font-size-lgx);
    line-height: var(--seq-font-line-height-lgx);
  }


  &-default {
    border: 1px solid var(--seq-color-stroke-divider-2);
    background: var(--seq-color-bg-index-1);
    color: inherit;
  }

  &-close {
    color: var(--seq-color-text-tertiary);
    cursor: pointer;

    &:hover {
      color: var(--seq-color-text-primary);
    }
  }

  &.is-disabled {
    cursor: not-allowed;

    .seq-tag-close {
      cursor: inherit;
      color: var(--seq-color-text-disabled);
    }
  }
}
</style> 