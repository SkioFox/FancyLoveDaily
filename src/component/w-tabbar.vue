<template>
    <view class="tabbar">
        <view class="tabbar-item" v-for="(item, index) in data.tabbarList" :key="index" @click="changeTabbar(item)">
        <img class="icon" :src="index == props.currentPage ? item.selectedIconPath : item.iconPath" />
        <view class="title" :style="index == props.currentPage ? { color: '#6cc2c1' } : ''">{{ item.text }}</view>
        </view>
    </view>
</template>
   
<script lang="ts" setup>
    import { reactive, defineProps, onMounted } from 'vue'
    
    const props = defineProps({
        currentPage: Number,
    })
    const data = reactive({
        tabbarList: [
            {
                "pagePath": "/pages/index/index",
                "iconPath": "../static/images/1-1.png",
                "selectedIconPath": "../static/images/1-2.png",
                "text": "邀请函"
            },
            {
                "pagePath": "/pages/photo/index",
                "iconPath": "../static/images/2-1.png",
                "selectedIconPath": "../static/images/2-2.png",
                "text": "甜蜜相册"
            },
            {
                "pagePath": "/pages/map/index",
                "iconPath": "../static/images/3-1.png",
                "selectedIconPath": "../static/images/3-2.png",
                "text": "酒店导航"
            },
            {
                "pagePath": "/pages/greet/index",
                "iconPath": "../static/images/4-1.png",
                "selectedIconPath": "../static/images/4-2.png",
                "text": "好友祝福"
            },
            {
                "pagePath": "/pages/message/index",
                "iconPath": "../static/images/5-1.png",
                "selectedIconPath": "../static/images/5-2.png",
                "text": "留言评论"
            }
        ]
    })

    const changeTabbar = function (item: any) {
        uni.switchTab({
            url: item.pagePath,
        })
    }

    onMounted(() => {
        uni.hideTabBar();
    })
</script>

<style lang="scss" scoped>
  .tabbar {
    position: fixed;
    left: 0;
    bottom: 0;
    display: flex;
    justify-content: space-around;
    width: 100%;
    box-sizing: border-box;
    /* padding: 15rpx 0rpx 55rpx 0rpx; */
    padding: 12rpx 0;
    background-color: #f8f8f8;
    border-top: #cfcfcf 1px solid;
    .tabbar-item {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
      .icon {
        width: 50rpx;
        height:50rpx;
      }
      .title {
        font-size: 16rpx;
      }
    }
  }
  </style>