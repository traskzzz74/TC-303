<template>
  <slot></slot>
</template>

<script>
export default {
  name: 'SeqSelectProvider',
  provide() {
    return {
      selectProvider: this
    }
  },
  data() {
    return {
      openedSelect: null
    }
  },
  methods: {
    setOpenedSelect(select) {
      if (this.openedSelect) {
        this.openedSelect.selected = false
      }
      this.openedSelect = select
    }
  }
}
</script>