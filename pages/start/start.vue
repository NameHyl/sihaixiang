<template>
	<view class="box">
		<image class="bg" src="../../static/w4.jpg"></image>
		<button open-type="getUserInfo" @click="getUserInfo" class="btn">登录授权</button>
	</view>
</template>

<script>
	import {baseUrl} from "@/api/apis.js"
	export default {
		data() {
			return {

			}
		},
		onLoad: () => {
			// 一进入页面 就应该获取本地存储的数据
			let info = uni.getStorageSync("userinfo");
			if(info){
				uni.switchTab({
					url: "/pages/index/index"
				})
			}
		},
		methods: {
			getUserInfo() {
				// 第一步： 获取用户头像和名字
				uni.getUserProfile({
					desc: "你是不是想授权,我帮你哦",
					provider: 'weixin',
					success: function(res) {
						console.log(res.userInfo);
						// 获取到用户数据后，马上存入本地
						uni.setStorageSync("userinfo",res.userInfo);
						// 跳转到首页 直接跳转到tabbar页面
						uni.switchTab({
						    url: '/pages/index/index'
						});
					}
				});
				// 第二步， 获取code传给后台 拿到openid(用户id)和session_key(后台的登录识别码)
				// code 是每一次登录的 登录凭证 每一次登录都不一样
				uni.login({
				  provider: 'weixin',
				  success: function (res) {
					console.log(res.code);
					if( res.code ){
						// 获取openid和session_key
						uni.request({
						    url: baseUrl + '/login/getOpenid', //仅为示例，并非真实接口地址。
						    data: {
						        code: res.code
						    },
							method: "POST",
						    success: (res) => {
						        console.log(res.data.data);
								uni.setStorageSync("info",res.data.data);
						    }
						});
					}
				  }
				})
			},
		}
	}
</script>

<style>
	.box,
	.bg,
	page {
		width: 100%;
		height: 100%;
		position: relative;
	}

	.btn {
		width: 150px;
		height: 50px;
		position: absolute;
		left: 50%;
		margin-left: -75px;
		bottom: 40px;
		background: #41A863;
		color: #fff;
	}
</style>
