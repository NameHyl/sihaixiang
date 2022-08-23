<template>
	<view>
		<view class="top" @click="getUser">
			<view>
				<view>{{ info.nickName }}</view>
				<view>欢迎您光临四海香</view>
			</view>
			<image class="avtorImg" :src="info.avatarUrl" mode=""></image>
		</view>
		<view class="btm">
			<navigator v-for="(item,i) in navlist" :key="i" class="navItem" :url="item.path">
				<view class="">
					<image class="icon" :src="'../../static/'+item.icon+'.png'" mode=""></image>
					<text>{{ item.text }}</text>
				</view>
				<view>></view>
			</navigator>
		</view>
		<button open-type="share">点我分享</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navlist: [{
						path: "/pagesA/myBag/myBag",
						icon: "icon_order",
						text: "烹饪历史"
					},
					{
						path: "/pages/report/report",
						icon: "icon_money",
						text: "支付记录"
					},
					{
						path: "/pages/report/report",
						icon: "icon_msg",
						text: "投诉举报"
					},
				],
				info: {
					nickName: "游客登录",
					avatarUrl: "../../static/15.png"
				}
			}
		},
		methods: {
			// 点击头像 登录授权
			getUser() {
				let _this = this;
				uni.getUserProfile({
					desc: "描述",
					success(res) {
						console.log(res)
						// 获取数据存入本地 并重绘视图
						uni.setStorageSync("userinfo", res.userInfo)
						_this.info = res.userInfo;
					}
				})
			}
		},
		onLoad(options) {
			let info = uni.getStorageSync("userinfo");
			console.log(info)
			if (info) {
				this.info = info;
			}
		},
		// 分享
		onShareAppMessage(res) {
			// if (res.from === 'button') { // 来自页面内分享按钮
			// 	console.log(res.target)
			// }
			return {
				title: '我请大家喝咖啡',
				imageUrl: "../../static/man2.jpg",
				path: '/pages/person/person?id=123'
			}
		}
	}
</script>

<style>
	.top {
		width: 90%;
		height: auto;
		margin: 20px auto;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.avtorImg {
		width: 80px;
		height: 80px;
		border-radius: 50%;
	}

	.navItem {
		width: 100%;
		height: 40px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0px 15px;
		box-sizing: border-box;
		border-bottom: 1px solid #eee;
	}

	.icon {
		width: 20px;
		height: 20px;
	}
</style>
