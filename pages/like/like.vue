<template>
	<view class="like">
		<view class="top">
			<view>已收藏{{ list.length }}个餐宴</view>
			<view @click="clear">清空</view>
		</view>
		<view class="btm" v-if="list.length > 0">
			<!-- 模版 -->
			<view class="item" v-for="(item,i) in list" :key="i">
				<view class="imgbox">
					<image class="goodsImg" :src="item.img"></image>
					<view class="fireBox">
						{{ item.fire }}
					</view>
				</view>
				<view class="ctrlBox">
					<view class="left">
						<view class="name">
							{{ item.name }}
						</view>
						<view class="price">
							￥{{ item.price }}/桌
						</view>
					</view>
					<view class="right" @click="del(item.id)">
						<image class="icon" src="../../static/footer2.png" mode=""></image>
						<view class="">
							取消收藏
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
				pageSize: 1,
				pageNum: 5,
				ischecked: true, // 定义一个开关,可以加载新的数据
				list: [
					{ id: 1, img: "../../static/banner.png", name: "麻辣小龙虾", price: 99, fire: 2001 },
					{ id: 2, img: "../../static/goods1.jpg", name: "蒜蓉小龙虾", price: 98, fire: 2002 },
					{ id: 3, img: "../../static/goods2.jpg", name: "五香大龙虾", price: 199, fire: 2030 },
					{ id: 4, img: "../../static/goods3.jpg", name: "香辣小龙虾", price: 95, fire: 2004 },
					{ id: 5, img: "../../static/goods1.jpg", name: "小龙虾刺身", price: 96, fire: 2005 },
				],
			}
		},
		methods: {
			// 点击取消收藏
			del(id){
				let _this = this;
				uni.showModal({
				    title: '取消收藏',
				    content: '确定取消吗?',
					confirmColor: "#f01",
				    success: function (res) {
				        if (res.confirm) {
				            // 循环遍历 找到相同的id 如果传递的id和遍历的id相同,删之
				            _this.list.forEach((item,i) => {
				            	if( id == item.id ){
				            		_this.list.splice(i,1);
				            	}
				            })
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
				
			},
			// 点击清空
			clear(){
				let _this = this;
				uni.showModal({
				    title: '清空收藏',
				    content: '清空收藏后,将无法恢复',
					confirmColor: "#f01",
				    success: function (res) {
				        if (res.confirm) {
				            console.log('用户点击确定');
							_this.list = [];
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			}
		},
		// 滚动到底部事件
		onReachBottom() {
			console.log("我是有底线的")
			// 发请求后,得到一个新的数组
			let newArr = [
				{ id: 6, img: "../../static/banner.png", name: "麻辣小龙虾1", price: 99, fire: 201 },
				{ id: 7, img: "../../static/goods1.jpg", name: "蒜蓉小龙虾1", price: 98, fire: 202 },
				{ id: 8, img: "../../static/goods2.jpg", name: "五香大龙虾1", price: 199, fire: 230 },
				{ id: 9, img: "../../static/goods3.jpg", name: "香辣小龙虾1", price: 95, fire: 204 }
			];
			// 要先判断 开关 如果为true 就可以继续加载 如果flase就停掉
			if( this.ischecked ){
				uni.showToast({
				    title: '加载中',
					icon: "loading",
				    duration: 2000
				});
				// 把新数组要和老数组合并一下 重绘视图
				this.list = this.list.concat(newArr);				
			}else{
				uni.showToast({
				    title: '到底了',
					icon: "loading",
				    duration: 2000
				});
			}
			// 如果新数组长度 小于5条,就不能再继续往下了
			if(newArr.length < this.pageNum){
				this.ischecked = false
			}
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
