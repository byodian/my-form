<template>
  <my-form :model="form" :rules="rules" ref="myForm">
    <my-form-item label="登录" prop="name">
      <div class="example">
        <my-input 
          v-model="form.name" 
          placeholder="请输入账号"></my-input>
      </div>
    </my-form-item>
    <my-form-item label="密码" prop="password">
      <my-input 
        v-model="form.password" 
        type="password" 
        placeholder="请输入密码"></my-input>
    </my-form-item>
    <my-form-item>
      <my-button @click="handleClick">登录</my-button>
    </my-form-item>
  </my-form>
</template>

<script>
import { MyButton, MyFormItem, MyInput, MyForm } from './components'

export default {
  name: 'App',
  componentName: 'App',
  components: {
    MyForm,
    MyFormItem,
    MyInput,
    MyButton,
  },
  data() {
    return {
      form: {},
      rules: {
        name: [
          { required: true, message: '账号不能为空', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码不能为空', tirgger: 'blue' }
        ]
      }
    }
  },
  mounted() {
    console.log(this.$refs.myForm)
  },
  methods: {
    handleClick() {
      this.$refs.myForm.validate((valid) => {
        if (valid) {
          console.log('submit')
        } else {
          console.log('error submit')
          return false
        }
      })
    }
  }
}
</script>

<style scoped>
</style>