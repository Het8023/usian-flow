<template>
  <view>
    <uni-forms ref="seedForm" :model="formData" :rules="rules">
      <uni-forms-item name="account">
        <input type="text" v-model="formData.account" placeholder="请输入账号" />
      </uni-forms-item>
      <uni-forms-item name="password">
        <!-- <input class="input" v-model="formData.email" type="password" placeholder="请输入密码" /> -->
        <uni-easyinput
          type="password"
          placeholderStyle="font-size: 32rpx"
          :clearable="true"
          :inputBorder="false"
          v-model="formData.password"
          placeholder="请输入密码"
        ></uni-easyinput>
      </uni-forms-item>
    </uni-forms>
    <button @click="submit" type="warn">登录</button>
  </view>
</template>

<script lang="ts" setup>
import { onReady } from '@dcloudio/uni-app'
import { ref } from 'vue'
import { login } from '@/api/login'
const seedForm = ref()
const resetForm = () => {
  return {
    account: 'xbsj001',
    password: '123456'
  }
}
let formData = ref(resetForm())
const rules = {
  // 对name字段进行必填验证
  account: {
    rules: [
      {
        required: true,
        errorMessage: '请输入账号'
      }
    ]
  },
  // 对email字段进行必填验证
  password: {
    rules: [
      {
        required: true,
        errorMessage: '请输入正确的密码'
      },
      {
        minLength: 6,
        maxLength: 10,
        errorMessage: '密码长度在 {minLength} 到 {maxLength} 个字符'
      }
    ]
  }
}
onReady(() => {
  seedForm.value.setRules(rules)
})
const submit = async () => {
  console.log(formData.value)
  seedForm.value.validate().then(async ({ valid }: any) => {
    if (valid) {
      console.log('校验失败')
    } else {
      //验证成功
      console.log('校验成功')
      login({
        ...formData.value
      }).then((res:any) => {
        console.log(res)
        if(res.data.code == 200){
          uni.showToast({
            title:"登陆成功",
            icon:"success"
          })
          localStorage.setItem('token',res.data.data)
          uni.navigateTo({
            url:"/pages/index/index"
          })
        }else{
          uni.showToast({
            title:res.data.msg,
            icon:"error"
          })
        }
      })
    }
  })
}
</script>

<style lang="scss">
.uni-forms-item {
  border-bottom: 1px solid #ccc;
}
</style>
