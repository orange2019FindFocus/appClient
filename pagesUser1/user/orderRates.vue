<template>
	<view class="uni-page-body">
		<view class="" v-if="listData.count">
      
			<view class="uni-bg-white uni-common-pt uni-common-pb uni-common-mt-md" v-for="(item,index) in listData.list" :key="index">
			  
			  <view class="uni-flex uni-common-pl uni-common-pr">
			  	<view class="" style="width: 80upx;height: 80upx;">
			  		<image lazy-load="true"  :src="userInfo.avatar" mode="" style="width: 80upx;height: 80upx;border-radius: 80upx;"></image>
			  	</view>
			    <view class="uni-flex-item uni-h4 uni-common-pl uni-text-blue">
			    	<text style="">{{ userInfo.nickname}}</text>
			    </view>
			    <view class="uni-flex-item uni-text-gray uni-right">
			    	<text style="" v-if="item.rate_level">
              {{ item.rate_date }}
            </text>
						<text v-else @tap="goRate(item)" class="btn-rate">去评价</text>
			    </view>
			  </view>
			  
			  <view class="uni-common-mt-sm uni-common-pl uni-common-pr">
			  	{{ item.rate_info }}
			  </view>
			  
			  <view class="uni-common-mt uni-common-pl" v-if="item.rate_imgs.length">
			  	<view class="uni-common-pr" style="display: inline-block;width: 100upx;height: 100upx;" v-for="(img,index1) in item.rate_imgs" :key="index1" @tap="preImg(item.rate_imgs, img)">
			  		<image lazy-load="true"  :src="img" mode="scaleToFill" style="width: 100upx;height: 100upx;border-radius: 4upx;border: 1px solid #EEEEEE;"></image>
			  	</view>
			  </view>
        
        <view class="uni-common-pl uni-common-pr">
        	<view class="uni-common-pa-md uni-bg-gray uni-common-mt-sm uni-flex">
        		<view class="" style="width: 200upx;height: 200upx;">
        			<image lazy-load="true"  :src="item.goods_cover" mode="scaleToFill" style="width: 200upx;height: 200upx;border-radius: 8upx;"></image>
        		</view>
            <view class="uni-flex-item uni-common-pl">
            	<view class="uni-ellipsis-2 uni-bold" style="height: 100upx;line-height: 50upx;">
            		{{ item.goods_title }}
            	</view>
              <view class="">
              	<text class="uni-text-light">商品购买价格:</text>
								<view class="uni-inline-block uni-text-red">
									<money :num="item.goods_amount" size="36" />
								</view>
                
              </view>
              <view class="uni-text-light uni-text-small">
              	购买完成时间：{{ item.create_date}}
              </view>
            </view>
        	</view>
        </view>
			  
			  <view class="uni-flex uni-common-pl uni-common-pr uni-common-mt " v-if="item.rate_level > 0">
          <view class="uni-inline-block" v-for="(level,index2) in levels" :key="index2">
          	<view v-if="level <= item.rate_level">
          		<!-- <uni-icon type="star-filled" size="30" color="#e03c45" ></uni-icon>
               -->
               <image src="/static/icon/user/start-a.png" mode="" style="width: 60upx;height: 60upx;margin-right: 30upx;"></image>
          	</view>
          	<view v-else>
          		
               <image src="/static/icon/user/start.png" mode="" style="width: 60upx;height: 60upx;margin-right: 30upx;"></image>
          	</view>
          	
          </view>
			  </view>
				
			</view>
      
		</view>
		
		<view class="uni-common-pa uni-center" v-else>无数据</view>
    
    <view class="uni-loadmore" v-if="showLoadMore">{{loadMoreText}}</view>
	</view>
</template>

<script>
  import {mapState,mapActions} from 'vuex';
  import money from '@/components/money.vue';
  import uniIcon from '@/components/uni-icon.vue';
  export default {
		components:{
			money,
      uniIcon
		},
    data(){
      return {
        listData:{
          page:1,
          count:0,
          list:[]
        },
        levels:[1,2,3,4,5],
        showLoadMore: false,
        loadMoreText:'加载中...',
      }
    },
    computed:{
      ...mapState(['userInfo'])
    },
    methods:{
			goRate(item){
				uni.navigateTo({
					url:'/pagesUser1/user/orderRatePost?id=' + item.id
				})
			},
      preImg(paths, current){
        uni.previewImage({
            urls: paths,
            current: current,
            indicator:'number'
        });
      },
      async getData(){
        let params = {}
        params.page = this.listData.page
        params.rate = 1
        
        let ret = await this.$store.dispatch('mallOrderItemListGet' , params)
				console.log('mallOrderItemListGet ret' , ret)
        if(ret.code == 0){
          let data =ret.data
          this.listData.page += 1
          this.listData.count = data.count
          
          let rows = data.rows
          if(rows.length == 0){
            this.loadMoreText = '无更多'
          }
          
          rows.forEach(row => {
            this.listData.list.push(row)
          })
        }
      },
      async refresh(){
        uni.startPullDownRefresh({
        	success: async () => {
        		this.listData.page = 1
        		this.listData.count = 0
        		this.listData.list = []
        		
        		await this.getData()
        		uni.stopPullDownRefresh()
        	}
        })
      }
    },
    onLoad() {
    	this.getData()
    },
    async onPullDownRefresh() {
    	this.listData.page = 1
    	this.listData.count = 0
      this.listData.list = []
      
      await this.getData()
      uni.stopPullDownRefresh()
    },
    async onReachBottom() {
    	this.showLoadMore = false
      await this.getData()
      this.showLoadMore = true
    },
    onShow() {
    	if(this.$store.state.userDataRefresh){
        this.refresh()
        this.$store.state.userDataRefresh = false
      }
    }
  }
</script>

<style>
  .btn-rate {
    padding: 8upx 16upx;
    border: 1px solid #d81e06;
    color: #D81E06;
    border-radius: 32upx;
  }
</style>
