<template>
  <div class="my-form-item">
    <div class="my-form-item__label" v-if="label">
      <label>{{ label }}</label>
    </div>
    <div class="my-form-item__content">
      <slot></slot>
      <transition name="zoom-in-top">
        <slot
          v-if="validateState === 'error'"
          name="error">
          <div class="my-form-item__error">
            {{ validateMessage }}
          </div>
        </slot>
      </transition>
    </div> 
  </div>
</template>

<script>
import Schema from 'async-validator'
export default {
  name: 'MyFormItem',
  componentName: 'MyFormItem',
  props: {
    label: String,
    prop: String
  },
  inject: ['myForm'],
  provide() {
    return {
      myFormItem: this
    }
  },
  data() {
    return {
      isNested: false,
      validator: {},
      validateMessage: '',
      validateState: ''
    }
  },
  computed: {
    form() {
      let parent = this.$parent
      let parentName = parent.$options.componentName
      while (parentName !== 'MyForm') {
        parent = parent.$parent
        parentName = parent.$options.componentName
      }
      return parent
    },
    fieldValue() {
      const model = this.form.model
      if (!model || !this.prop) {
        return
      }
      return model
    }
  },
  mounted() {
    if (this.prop) {
      this.$parent.$emit('my.form.addField', this)
      this.addValidateEvents()
    }
  },
  methods: {
    validate(trigger, callback = () => {}) {
      const rules = this.form.rules[this.prop] || []
      if (!rules || rules.length === 0) {
        callback()
        return true
      }

      const descriptor = {}
      rules.forEach(rule => {
        if (rule.trigger === trigger || trigger === '') {
          delete rule.trigger
        }
      })
      descriptor[this.prop] = rules

      let model = {}
      if (this.fieldValue) {
        model = this.fieldValue
      } else {
        model[this.prop] = this.fieldValue
      }

      const validator = new Schema(descriptor)
      validator.validate(model, (errors, invalidFields) => {
        this.validateState = !errors ? 'success' : 'error';
        this.validateMessage = errors ? errors[0].message : '';
        callback(this.validateMessage, invalidFields);
      })
    },
    onFieldBlur() {
      this.validate('blur')
    },
    addValidateEvents() {
      if (this.form.rules) {
        this.$on('my.form.blur', this.onFieldBlur)
      }
    },
  }
}
</script>

<style>
.my-form-item {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 30px;
}
.my-form-item__label {
  height: 40px;
  line-height: 40px;
  padding: 0 12px 0 0;
}
.my-form-item__content {
  position: relative;
}

.my-form-item__error {
  position: absolute;
  top: 100%;
  color: red;
  font-size: 14px;
}
</style>