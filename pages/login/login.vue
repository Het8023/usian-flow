<template>
  <view class="user-login">
    <view class="login-type">
      <view class="title">
        {{ tabMeta.title }}
      </view>
      <view class="type">
        <text @click="handleChangeType">{{ tabMeta.subTitle }}</text>
        <text class="iconfont icon-caret"></text>
      </view>
    </view>
    <!-- 手机号登录 -->
    <Iaccount v-if="tabIndex !== 0"></Iaccount>
    <!-- 账号登录 -->
    <Imobile v-else></Imobile>
  </view>
</template>

<script setup lang="ts">
import { metaType } from './types/type'
import { ref, computed } from 'vue'
import Iaccount from './components/account.vue'
import Imobile from './components/mobile.vue'

// 控制默认选中的状态 0手机号登录 1账号登录
const tabIndex = ref<number>(0)
// 定义的元数据
const tabMetas = ref<metaType[]>([
  {
    title: '手机号登录',
    subTitle: '账号登录'
  },
  {
    title: '账号登录',
    subTitle: '手机号登录'
  }
])
// 获取默认数据
const tabMeta = computed(() => {
  return tabMetas.value[tabIndex.value]
})
// 切换类型
const handleChangeType = () => {
  tabIndex.value = Math.abs(tabIndex.value - 1)
}
</script>

<style lang="scss">
.user-login {
  padding: 66rpx;

  .login-type {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    margin: 40rpx 0 80rpx;
    line-height: 1;

    .title {
      font-size: 48rpx;
      color: #2a2929;
    }

    .type {
      color: #ef4f3f;
      font-size: 32rpx;
      display: flex;
      align-items: center;
    }

    .icon-caret {
      font-size: 50%;
      margin-left: 10rpx;
    }
  }
}
</style>
