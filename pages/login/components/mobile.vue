<template>
  <view>
    <uni-forms ref="seedForm" :model="formData" :rules="rules">
      <uni-forms-item name="name">
        <input type="text" v-model="formData.name" placeholder="请输入手机号" />
      </uni-forms-item>
      <uni-forms-item name="email">
        <view class="a1">
          <input class="input" v-model="formData.email" type="text" placeholder="请输入验证码" />
          <view class="yzm" @click="hq">获取验证码</view>
        </view>
      </uni-forms-item>
    </uni-forms>
    <button @click="submit" type="warn">登录</button>
  </view>
</template>

<script lang="ts" setup>
import { onReady } from '@dcloudio/uni-app'
import { ref } from 'vue'
import { captcha } from '@/api/login'
const seedForm = ref()
const resetForm = () => {
  return {
    name: '',
    email: ''
  }
}
let formData = ref(resetForm())
const rules = {
  // 对name字段进行必填验证
  name: {
    rules: [
      {
        required: true,
        errorMessage: '请输入手机号'
      }
    ]
  },
  // 对email字段进行必填验证
  email: {
    rules: [
      {
        required: true,
        errorMessage: '请输入验证码'
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
    }
  })
}

// 获取验证码
const hq = () => {
  captcha({
    key: "12345678"
  }).then((res: any) => {
    console.log(res)
  })
}
</script>

<style lang="scss">
.a1 {
  display: flex;
  justify-content: space-between;
}
.uni-forms-item {
  border-bottom: 1px solid #ccc;
}
</style>
