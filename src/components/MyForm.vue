<template>
  <form class="my-form">
    <slot></slot>
  </form>
</template>

<script>
export default {
  name: 'MyForm',
  componentName: 'MyForm',
  props: {
    model: Object,
    rules: [Object, Array]
  },
  provide() {
    return {
      myForm: this
    }
  },
  data() {
    return {
      fields: []
    }
  },
  created() {
    this.$on('my.form.addField', (field) => {
      if (field) {
        this.fields.push(field);
      }
    });
  },
  mounted() {
    console.log(this.fields)
  },
  methods: {
    validate(callback) {
      if (!this.model) {
        console.warn('[Element Warn][Form]model is required for validate to work!');
        return;
      }

      let promise
      if (typeof callback !== 'function' && window.Promise) {
        promise = new window.Promise((resolve, reject) => {
          callback = function(valid) {
            valid ? resolve(valid) : reject(valid)
          }
        })
      }

      let valid = true
      let count = 0

      if (this.fields.length === 0 && callback) {
        callback(true)
      }

      // let invalidFields = {};
      this.fields.forEach(field => {
        field.validate('', (message) => {
          if (message) {
            valid = false;
          }
          // invalidFields = Object.assign({}, invalidFields, field);
          if (typeof callback === 'function' && ++count === this.fields.length) {
            callback(valid);
          }
        });
      });

      if (promise) {
        return promise
      }
    }
  }

}
</script>

<style>
.my-form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>