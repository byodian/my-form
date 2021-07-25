<template>
  <button
    class="my-button"
    @click="handleClick"
    :class="[
      type ? 'my-button--' + type : '',
    ]">
    <slot></slot> 
  </button>
</template>

<script>
export default {
  name: 'MyButton',
  props: {
    type: String
  },
  methods: {
    handleClick(e) {
      this.$emit('click', e)
    }
  }
}
</script>

<style>

</style>