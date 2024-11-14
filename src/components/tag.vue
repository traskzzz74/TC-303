<template>
  <span 
    class="seq-tag"
    :class="[
      `seq-tag-${size}`,
      `seq-tag-${type}`,
      {
        'is-closable': closable
      }
    ]"
  >
    <slot></slot>
    <iconpark-icon 
      v-if="closable"
      name="close"
      class="seq-tag-close"
      @click.stop="handleClose"
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
  border-radius: var(--seq-radius-sm);
  font-weight: normal;
  line-height: 1;
  white-space: nowrap;

  &-sm {
    height: 20px;
    font-size: var(--seq-font-size-sm);
  }
  &-md {
    height: 24px;
    font-size: var(--seq-font-size-md);
  }
  &-lg {
    height: 28px;
    font-size: var(--seq-font-size-lg);
  }

  &-default {
    color: var(--seq-color-text-regular);
    background-color: var(--seq-color-primary-light);
  }
  &-primary {
    color: var(--seq-color-white);
    background-color: var(--seq-color-primary);
  }
  &-success {
    color: var(--seq-color-white);
    background-color: var(--seq-color-success);
  }
  &-warning {
    color: var(--seq-color-white);
    background-color: var(--seq-color-warning);
  }
  &-danger {
    color: var(--seq-color-white);
    background-color: var(--seq-color-danger);
  }

  &-close {
    margin-left: var(--seq-spacing-xs);
    font-size: 12px;
    cursor: pointer;
    transition: color var(--seq-transition-duration) var(--seq-transition-timing-function);

    &:hover {
      color: var(--seq-color-text-placeholder);
    }
  }
}
</style> 