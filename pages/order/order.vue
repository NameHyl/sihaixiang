<template>
	<view class="like">
		<view class="top">
			<view>已有{{ list.length }}个行程</view>
			<view>历史</view>
		</view>
		<view class="btm" v-if="list.length > 0">
			<!-- 模版 -->
			<view class="item" v-for="(item,i) in list" :key="i">
				<view class="">
					{{ item.time }}
				</view>
				<view class="">
					{{ item.address }}
				</view>
				<view class="imgbox">
					<image class="goodsImg" :src="item.img"></image>
				</view>
				<view class="ctrlBox">
					<view class="left">
						<view class="name">
							{{ item.goodsname }}
						</view>
						<view class="price">
							￥{{ item.price }}/桌
						</view>
					</view>
					<view class="right" @click="callMe(item.telname,item.tel)">
						<image class="icon" src="../../static/93.png" mode=""></image>
						<view class="">
							拨打电话
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 没有数据的时候 -->
		<view class="nomore" v-else>
			<image class="noImg" src="../../static/nomore2.png"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [
					{ orderid: 1, img: "../../static/banner.png", goodsname: "麻辣小龙虾", price: 99, address: "天府新谷1号楼6楼", time: "07-10 周六 下午 送货上门", telname: "王大厨",tel: 15883742135 },
					{ orderid: 2, img: "../../static/goods1.jpg", goodsname: "蒜蓉小龙虾", price: 98, address: "天府新谷1号楼7楼", time: "07-10 周六 下午 送货上门", telname: "王鑫",tel: 18780562542  },
					{ orderid: 3, img: "../../static/goods2.jpg", goodsname: "五香大龙虾", price: 199, address: "天府新谷1号楼8楼", time: "07-10 周六 下午 送货上门", telname: "唐大厨",tel: 18280066970  },
					{ orderid: 4, img: "../../static/goods3.jpg", goodsname: "香辣小龙虾", price: 95,address: "天府新谷1号楼5楼", time: "07-10 周六 下午 送货上门", telname: "金大厨",tel: 18800960298  },
					{ orderid: 5, img: "../../static/goods1.jpg", goodsname: "小龙虾刺身", price: 96, address: "天府新谷1号楼9楼", time: "07-10 周六 下午 送货上门", telname: "姜大厨",tel: 17760347065  },
				],
			}
		},
		methods: {
			callMe(name,tel){
				console.log(name,tel);
				uni.showModal({
				    title: name,
				    content: tel+"",
					confirmText: "拨打电话",
					cancelText: "我就看看",
				    success: function (res) {
				        if (res.confirm) {
				            console.log('用户点击确定');
							uni.makePhoneCall({
							    phoneNumber: tel+"" //仅为示例
							});
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			}
			
		},
		// 滚动到底部事件
		onReachBottom() {
		}
	}
</script>

<style>
.like{
	width: 100%;
	height: auto;
}
.top{
	width: 100%;
	height: 40px;
	display: flex;
	padding: 0px 15px;
	box-sizing: border-box;
	justify-content: space-between;
	align-items: center;
}
.item{
	width: 90%;
	height: auto;
	margin: 10px auto;
	border-radius: 7px;
	box-shadow: 1px 1px 10px 1px #ccc;
	overflow: hidden;
}
.imgbox{
	width: 100%;
	height: 200px;
	position: relative;
}
.goodsImg{
	width: 100%;
	height: 100%;
}
.fireBox{
	position: absolute;
	right: 20px;
	bottom: 20px;
	background: rgba(0,0,0,0.6);
	color: #fff;
	width: 80px;
	height: 30px;
	text-align: center;
	line-height: 30px;
	border-radius: 7px;
}
.ctrlBox{
	width: 90%;
	height: auto;
	margin: 5px auto;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.right{
	text-align: center;
}
.icon{
	width: 30px;
	height: 30px;
}
.nomore{
	width: 100%;
	margin-top: 90px;
	text-align: center;
}
.noImg{
	width: 150px;
	height: 130px;
}

</style>
