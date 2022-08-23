<template>
	<view class="car">
		<view class="list">
			<scroll-view scroll-y class="left">
				<view v-for="(item,i) in arr" :key="i" :class="showTitle == i?'leftItem cur':'leftItem'" @click="changeTitle(i)">
					{{ item.title }}
				</view>
			</scroll-view>
			
			<view style="flex: 1; overflow: scroll;">
				<block v-for="(item,i) in arr" :key="i" v-if="showTitle == i">
					<!-- 循环第一层 -->
					<scroll-view scroll-y class="right">
						<view class="rightBox">
							<!-- 标题 -->
							<view class="title">{{ item.title }}</view>
							<!-- 循环第二层 -->
							<view class="rightItem" v-for="(obj,j) in item.child" :key="j">
								<image class="goodsImg" :src="obj.imgs"></image>
								<view class="ctrlBox">
									<view class="goodsName">
										{{ obj.name }}
									</view>
									<view class="price">
										￥{{ obj.price }}一瓶
									</view>
									<view class="ctrl">
										<view class="btn" @click="add(obj,-1,obj.id)">-</view>
										<text>{{ obj.num }}</text>
										<view class="btn" @click="add(obj,1,obj.id)">+</view>
									</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</block>
			</view>
			
		</view>
		<view class="btm">
			<view>
				{{ total }}
			</view>
			<view>
				{{ totalPrice }}
			</view>
			<view class="payBtn" @click="goPay">
				支付
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showTitle: 0,
				selArr: [],
				arr: [
					{title: "白酒",child: [
						{id: 1, name: "五粮液", price: 998,num: 0, imgs: '../../static/goods1.jpg'},
						{id: 2, name: "茅台", price: 2999,num: 0, imgs: '../../static/goods2.jpg'},
						{id: 3, name: "歪嘴", price: 15,num: 0, imgs: '../../static/goods3.jpg'},
						{id: 4, name: "牛栏山", price: 20,num: 0, imgs: '../../static/goods1.jpg'},
						{id: 5, name: "剑南春", price: 200,num: 0, imgs: '../../static/goods2.jpg'},
					]},
					{title: "啤酒", child: [
						{id: 6, name: "雪花", price: 9,num: 0, imgs: '../../static/goods1.jpg'},
						{id: 7, name: "勇闯天涯", price: 15,num: 0, imgs: '../../static/goods2.jpg'},
						{id: 8, name: "青岛啤酒", price: 10,num: 0, imgs: '../../static/goods3.jpg'},
						{id: 9, name: "百威", price: 12,num: 0, imgs: '../../static/goods1.jpg'},
						{id: 10, name: "夺命大乌苏", price: 20,num: 0, imgs: '../../static/goods2.jpg'},
					]}
				]
			}
		},
		methods: {
			// 点击分类 切换
			changeTitle(i){
				this.showTitle = i;
			},
			// 支付
			goPay(){
				uni.requestPayment({
				    provider: 'wxpay',  // 提供的服务商
				    timeStamp: '1536378375',  // 时间戳
				    nonceStr: 'b8a7e04f54723c35e75d2b27109dc2be', // 随机串
				    package: 'prepay_id=wx081146152548827173cee9463972166291',  // 配置选项
				    signType: 'MD5',  // 签名算法
				    paySign: '6815C06D715FC4B9FD4E1FA2E9B233FD',  // 签名
				    success: function (res) {
						// 5. 支付完成后,会有一个成功回调.
				        console.log('success:' + JSON.stringify(res));
				    },
				    fail: function (err) {
				        console.log('fail:' + JSON.stringify(err));
				    }
				});
				// 1. 拿到数组 和 openid 发给后台
				// uni.request({
				// 	url: "/api/v1/order/place",
				// 	data: {商品数组,openid},
				// 	method: "POST",
				// 	success(res) {
				// 		// 2. 后台会生成一个订单 (详情 id)
				// 		// res中 包含订单详情和订单id
				// 		// 3. 后台会根据前端传递过来的订单id,会返回给前端调起微信支付的必要参数
				// 		uni.request({
				// 			url: "/pay/pre_order",
				// 			data: { openid, 订单id: res中},
				// 			method: "POST",
				// 			success(res){
				// 				//  这个res中就会包含 前端调起微信支付的必要参数 时间戳,随机串,签名,签名算法
				// 				// 4. 前端拿到后台返的必要参数,调起微信支付的api
				// 				uni.requestPayment({
				// 				    provider: 'wxpay',  // 提供的服务商
				// 				    timeStamp: String(Date.now()),  // 时间戳
				// 				    nonceStr: 'A1B2C3D4E5', // 随机串
				// 				    package: 'prepay_id=wx20180101abcdefg',  // 配置选项
				// 				    signType: 'MD5',  // 签名算法
				// 				    paySign: '',  // 签名
				// 				    success: function (res) {
				// 						// 5. 支付完成后,会有一个成功回调.
				// 				        console.log('success:' + JSON.stringify(res));
				// 				    },
				// 				    fail: function (err) {
				// 				        console.log('fail:' + JSON.stringify(err));
				// 				    }
				// 				});
				// 			}
				// 		})
				// 	}
				// })
			},
			// 点击加号 减号
			add(obj,num,id){
				console.log(obj);
				console.log(num);
				obj.num += num;
				
				// 循环遍历 如果传递的id和遍历的id相同,说明找到数据 去重
				for( let i=0; i<this.selArr.length;i++ ){
					if( id == this.selArr[i].id ){
						this.selArr.splice(i, 1);
					}
				}
				this.selArr.push(obj);
				console.log(this.selArr)
			}
		},
		computed: {
			total(){
				let total = 0;
				for( let obj of this.selArr ){
					total += obj.num;
				}
				return total;
			},
			totalPrice(){
				let totalPrice = 0;
				for( let obj of this.selArr ){
					totalPrice += obj.num * obj.price
				}
				return totalPrice;
			}
		}
	}
</script>

<style>
page{
	width: 100%;
	height: 100%;
}
.car{
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
}
.list{
	flex: 1;
	display: flex;
	overflow: scroll;
}
.left{
	width: 100px;
	height: 100%;
	overflow: scroll;
	background: #ccc;
}
.leftItem{
	width: 100%;
	padding: 15px;
	box-sizing: border-box;
}
.cur{
	background: #fff;
}
.right{
	flex: 1;
}
.rightItem{
	width: 90%;
	margin: 10px auto;
	display: flex;
}
.goodsImg{
	width: 80px;
	height: 80px;
}
.ctrlBox{
	flex: 1;
}
.ctrl{
	display: flex;
	justify-content: flex-end;
}
.btn{
	width: 20px;
	height: 20px;
}
.btm{
	width: 100%;
	height: 60px;
	background: orange;
	color: #fff;
	display: flex;
	align-items: center;
	justify-content: space-between;
}
.payBtn{
	width: 120px;
	height: 60px;
	line-height: 60px;
	text-align: center;
	background: #41A863;
}
</style>
