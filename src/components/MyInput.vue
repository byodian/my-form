<template>
  <div class="my-input">
    <input 
      class="my-input-inner"
      :type="type" 
      @input="handleInput"
      @blur="handleBlur"
      v-bind="$attrs">
  </div>
</template>

<script>
export default {
  props: {
    value: [String, Number],
    type: {
      type: String,
      default: 'text'
    }
  },
  inject: ['myFormItem'],
  data() {
    return {}
  },
  mounted() {
  },
  methods: {
    handleInput(e) {
      this.$emit('input', e.target.value)
    },
    handleBlur(e) {
      this.$emit('blur', e)
      this.myFormItem.$emit('my.form.blur', [this.value])
      // this.$parent.$emit('my.form.blur', [this.value])
    }
  }
}
</script>

<style>
.my-input-inner {
  border-radius: 2px;
  border: 1px solid #ddd;
  line-height: 40px;
  height: 40px;
  padding: 0 15px;
}
</style>