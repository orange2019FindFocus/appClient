<template>
	<view class="user-card uni-bg-white">
    <view class="user-card-header uni-common-pa uni-border-bottom uni-bold">
    	{{ title }}
    </view>
    <view class="user-card-body">
      <template v-if="items.length">
      	<view class="user-card-item" @tap="goTo(item.path)" v-for="(item,index) in items" :key="index">
          <view class="uni-common-pa">
          	<view class="user-card-item-img">
          		<image lazy-load="true"  :src="'/static/icon/user/' + item.icon + '.png'" mode="scaleToFill"></image>
          	</view>
          	<view class="uni-text-light">
          		{{item.text}}
          	</view>
          </view>
      	</view>
      </template>
    	
    </view>
  </view>
</template>
<script>

  export default {
    props:{
      title: String,
      items: Array,
      hasLogin: Boolean
    },
    methods:{
      goTo(path){
        if(!this.hasLogin){
          uni.navigateTo({
          	url:'/pages/auth/login'
          })
        }else {
           uni.navigateTo({
           	url:path,
             fail() {
             	console.log('跳转失败')
             }
           })
        }
        
      }
    }
  }
</script>

<style>
  .user-card-body {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    width: 100%;
  }
  .user-card-item {
    width: 25%;
    text-align: center;
  }
  .user-card-item-img {
   text-align: center;
  }
  
  .user-card-item-img image {
     width: 48upx;
    height: 48upx;
  }
</style>