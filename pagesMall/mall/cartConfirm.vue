<template>
	<view class="page">
    
    <view class="uni-border-top" v-if="!isVipOrder">
      <view v-if="jdOrder == 1">
        <user-address :address="userAddressCurrent" :modify="false"></user-address>
      </view>
      <view v-else>
      	<user-address :address="userAddressCurrent" v-if="userAddressCurrent.id"></user-address>
      	<user-address v-else ></user-address>
      </view>
    	
    </view>
    
    <view class="uni-common-mt-md uni-bg-white ">
    	<view class="" v-for="(list,index) in cartList" :key="index">
        
    		<view class="" v-if="list.length">
          
          <view class="uni-bold">
          	<view class="uni-common-pa" v-if="index == 1">
          		自营商城
          	</view>
          	<view class="uni-common-pa uni-border-top" v-if="index == 2">
          		京选商城
          	</view>
          </view>
    			
          
          <view class="uni-common-pl uni-common-pr" v-for="(item,index2) in list" :key="index2">
          	<view class="uni-flex uni-border-top uni-common-pt uni-common-pb">
          		<view class="" style="160upx;height: 160upx;">
          			<image lazy-load="true"  :src="item.cover" mode="scaleToFill" style="width: 160upx;height: 160upx;border-radius: 8upx;"></image>
          		</view>
              <view class="uni-flex-item uni-common-pl" >
              	<view class="uni-ellipsis-2 uni-bold" style="height: 80upx;line-height: 40upx;">
              		{{item.title}}
              	</view>
                <view class="uni-flex ">
                	<view class="uni-flex-item uni-text-small uni-text-gray">
                		<text>积分可抵扣:</text>
                		<!-- <text style="margin-left: 10upx;">￥</text> -->
                		<view class="uni-inline-block uni-text-small" >
                      <money :num="item.price_score_sell" v-if="!isVip"/> 
                      <money :num="item.price_score_vip" v-else/> 
                      <text class="uni-common-ml-sm"> x {{item.num}}</text>
                    </view>
                	</view>
                </view>
                <view class="uni-flex ">
                	<view class="uni-flex-item uni-text-red">
                    <money :num="item.price_sell" size="28" v-if="!isVip"/>
                    <money :num="item.price_vip" size="28" v-else/>
                	</view>
                  <view class="uni-flex-item uni-right uni-text-light">
                    <text>x</text>
                    <text style="margin-left: 10upx;">{{ item.num}}</text>
                  </view>
                </view>
              </view>
          	</view>
          </view>
          
    		</view>
        
        
    	</view>
    </view>
    
    <view class="uni-common-mt-md uni-common-pl uni-common-pr uni-bg-white uni-border-top" v-if="!isVipOrder">
      
			<view class="uni-flex uni-common-pt uni-common-pb uni-border-bottom " >
				<view class="uni-flex-item input-label">
					运费
				</view>
			  <view class="uni-flex-item uni-right">
			  	<money :num="mallOrderConfirm.priceExpress" size="28"/>
			  </view>
			</view>
			
    	<view class="uni-flex uni-common-pt uni-common-pb uni-border-bottom " >
    		<view class="uni-flex-item input-label">
    			积分余额
    		</view>
    	  <view class="uni-flex-item uni-right">
    	  	{{userInfo.score}}
    	  </view>
    	</view>
    	
    	<view class="uni-flex uni-common-pt uni-common-pb uni-border-bottom " @tap="scoreUse">
    		<view class="uni-flex-item input-label">
    			使用积分
    		</view>
    	  <view class="uni-flex-item uni-right" >
          <text style="margin-right: 10upx;">{{scoreNum}}</text> 
    	  	<uni-icon type="checkbox-filled" size="22" v-if="postData.score" color="#d81e06"></uni-icon>
    	  	<uni-icon type="circle" size="22" v-else></uni-icon>
    	  </view>
    	</view>
      
      <view class="uni-flex uni-common-pt uni-common-pb " >
      	
        <view class="uni-flex-item uni-right" >
					<view class="uni-inline-block uni-text-small uni-text-gray" v-if="postData.score">
						<text >
							积分抵扣:
						</text>
						<money :num="score" />
					</view>
          
          <text style="margin-left: 10upx;">小计</text>
        	<!-- <text class="uni-text-red">￥</text> -->
          <view class="uni-inline-block uni-text-red">
          	<money :nums="[total , mallOrderConfirm.priceExpress]" size="36"/>
          </view>
        </view>
      </view>
    </view>
		
		<view class="uni-common-pa uni-bg-white uni-right uni-border-top" v-else>
			<text style="margin-left: 10upx;">小计</text>
			<!-- <text class="uni-text-red">￥</text> -->
			<view class="uni-inline-block uni-text-red">
				<money :num="[total, mallOrderConfirm.priceExpress]" size="36"/>
			</view>
		</view>
    
    <view class="uni-common-mt-md uni-common-pl uni-common-pr uni-bg-white uni-border-top">
    	<view class="uni-flex uni-common-pt uni-common-pb">
    		<view class="uni-flex-item input-label">
    			发票信息
    		</view>
    	  <view class="uni-flex-item uni-right" @tap="invoiceChoose">
    	  	<view class="" v-if="!mallOrderConfirm.invoice">
    	  		不开发票
    	  	</view>
    	    <view class="" v-else>
            已选择
    	    	<uni-icon type="arrowright" size="22"></uni-icon>
    	    </view>
    	  </view>
    	</view>
    	
    	<view class="uni-flex uni-common-pt uni-common-pb uni-border-top" v-if="!isVipOrder">
    		<view class="input-label">
    			备注
    		</view>
    	  <view class="uni-flex-item uni-right uni-common-pa uni-bg-gray uni-text-small" >
    	  	<textarea type="text" placeholder="填写备注信息(非必填)" auto-height="true" style="width: 500upx;" v-model="postData.remark" placeholder-class="uni-text-small" maxlength="255"/>
    	  </view>
    	</view>
    </view>
    
    <view style="height: 120upx;">
    	
    </view>

    <view class="uni-fixed-bottom uni-center uni-bg-red" @tap="orderCreate">
    	提交
    </view>
	</view>
</template>

<script>
  import {
    mapState,
    mapActions
  } from 'vuex';
  import Cart from '@/static/js/cart.js';
  import userAddress from '@/components/user/user-address.vue';
  import uniIcon from '@/components/uni-icon.vue';
  import money from '@/components/money.vue';
	export default {
    data(){
      return {
        postData:{
          // pay_type:0,
          // pay_method:'',
          score:0,
          total:0,
          remark:''
        },
        cartList:[],
        address : {},
        // invoice:false
//         payTypes: [
//           {id: 1, name:'微信支付','pay_type':'3' , 'pay_method':'wx'},
//           {id: 2, name:'支付宝','pay_type':'3' , 'pay_method':'alipay'},
//           {id: 3, name:'代金券','pay_type':'1' , 'pay_method':'ecard'},
//           {id: 4, name:'账户余额','pay_type':'2' , 'pay_method':'balance'},
//         ],
        // payTypeId:0,
        score:0,
        scoreNum:0,
        total:0,
				type:0,
				isVipOrder:0,
        jdOrder:0
      }
    },
    components:{
      userAddress,
      uniIcon,
      money
    },
    computed:{
      ...mapState(['hasLogin','isVip', 'userInfo','userAddressList' , 'userAddressCurrent' , 'mallOrderConfirm', 'userInvoice']),
//       scoreNum(){
//         return this.score * this.$store.state.scoreExchangeNum
//       }
    },
		methods:{
      
      ...mapActions(['goToLoginPage', 'userInfoGet']),
      
			async orderCreate(){
        
        uni.showLoading({
        	title:'订单提交中'
        })
        
				// let cartList = Cart.listChecked()
				let cartList = this.cartList
        let orderDatas = []
        cartList.forEach((items,index) => {
          if(items.length){
            orderDatas.push({
              order_type:index,
              items: items
            })
          }
        })
        
        let postData = this.postData
        postData.orders = orderDatas
        postData.score = this.postData.score * this.$store.state.scoreExchangeNum
        postData.address = this.userAddressCurrent || {}
        postData.invoice = this.mallOrderConfirm.invoice ? this.userInvoice : ''
        postData.express_fee = this.mallOrderConfirm.priceExpress || 0
				
        console.log('orderCreate postData' , postData)
        
        if(!postData.address.id){
          uni.showToast({
          	title:'请选择收货地址',
            icon:'none',
            duration:2000
          })
          return
        }
        
        let ret = await this.$store.dispatch('mallOrderCreate' , postData)
        if(ret.code === 0){
          uni.hideLoading()
          let orderIds = ret.data.ids
          let totals = ret.data.totals
          this.$store.state.mallPayment.orderIds = orderIds
          this.$store.state.mallPayment.totals = parseFloat(parseFloat(totals).toFixed(2))
          
          console.log('this.$store.state.mallPayment' , this.$store.state.mallPayment)
					if(this.type == 0){
						// 消除购物车
						Cart.listCheckedClear()
					}
					
					let page = '/pagesMall/mall/payment'
					if(this.isVipOrder){
						page += '?isVipOrder=1'
					}
          uni.showToast({
          	title:'生成订单成功',
            duration:2000,
            success() {
              uni.redirectTo({
              	url:page
              })
            },
            icon:'success'
          })
        }else {
          uni.hideLoading()
          uni.showToast({
          	title:ret.message,
            icon:'none',
            duration:2000
          })
        }
    
			},
      invoiceChoose(){
        uni.showActionSheet({
        	itemList: ['不开发票', '填写发票信息'],
        	success: (e) => {
        		console.log(e.tapIndex);
        		if (e.tapIndex == 0) {
        			this.mallOrderConfirm.invoice = 0
        		} else{
        			uni.navigateTo({
        				url:'/pagesMain/user/invoice'
        			})
        		}
        	}
        })
      },
//       payTypeChoose(item){
//         this.postData.pay_type = item.pay_type
//         this.postData.pay_method = item.pay_method
//         this.payTypeId = item.id
//       },
      scoreUse(){
        if(this.score * this.$store.state.scoreExchangeNum > this.userInfo.score){
          uni.showToast({ title: '积分余额不足', icon: "none" })
          return 
        }
        
        this.postData.score = (this.postData.score == 0) ? 1: 0
				let total = 0
				let score = 0
				if(this.type == 0){
					total = this.isVip ? Cart.totalVip : Cart.total
					score = this.isVip ? Cart.scoreVip : Cart.score
				}else {
					let item = this.$store.state.cartListBuyItem
					score = this.isVip ? item.price_score_vip * item.num : item.price_score_sell * item.num
					total = this.isVip ? item.price_vip * item.num : item.price_sell * item.num 
				}
        
        if(this.postData.score){
          this.total = parseFloat(parseFloat(total).toFixed(2))
        }else{
          this.total = parseFloat(parseFloat(total + score).toFixed(2))
        }
      }
		},
    onLoad(opt) {
      
      console.log('onLoad ====================')
    	if(!this.hasLogin){
    	  this.goToLoginPage()
    	  return
    	}
      
      // 获取实时用户数据
      this.userInfoGet()
      
      if(!this.userAddressList.length){
        this.$store.dispatch('userAddressGet')
      }
      
      console.log('userAddressCurrent' , this.userAddressCurrent)
			
			let type = opt.type || 0 // 结算类型 0：购物车 1：直接购买
			let isVipOrder = opt.isVipOrder || 0 // 是否VIP充值订单
      console.log('onLoad======= isVipOrder :' + isVipOrder)
      if(type == 0){
				let cartInfo = Cart.info()
				
				console.log('cartInfo' , cartInfo)
				this.score = this.isVip ? cartInfo.scoreVip : cartInfo.score
        console.log('this.score cart =============' , this.score)
				this.total = this.isVip ? (cartInfo.totalVip  + this.score)  : (cartInfo.total  + this.score )
				console.log('this.score total =============' , this.total)      
				this.cartList = Cart.listChecked()
				
				console.log('cartList' , this.cartList)
			}else if (type == 1) {
				let item = this.$store.state.cartListBuyItem
				console.log('onLoad add item' , item)
				let datas = [[],[],[]]
				datas[item.type].push(item)
				this.cartList = datas
				
				this.score = this.isVip ? (item.price_score_vip * item.num) : (item.price_score_sell * item.num)
				this.total = this.isVip ? ((item.price_vip  + this.score)  * item.num ) : ((item.price_sell + this.score) * item.num)
			}
      
      console.log('onLoad this.cartList:' + JSON.stringify(this.cartList))
      this.scoreNum = parseInt(this.score * this.$store.state.scoreExchangeNum)
      this.score = parseFloat(parseFloat(this.score).toFixed(2))
      this.total = parseFloat(parseFloat(this.total).toFixed(2))
      
      console.log('this.total =============' , this.total)
			this.type = type
			this.isVipOrder = isVipOrder
      // 发票默认不选
      // this.$store.state.mallOrderConfirm.invoice = 0
      // this.address = this.userAddressCurrent
      
      // 京东订单
      this.jdOrder = opt.jdOrder || 0
    }
	}
</script>

<style>
  
</style>
