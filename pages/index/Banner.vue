<template>
	<view class="banner">
		<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
			:duration="duration">
			<swiper-item v-for="(item,i) in background">
				<image class="img" :src="item.ad_pic" />
			</swiper-item>
		</swiper>
		<navigator class="search">
			<image class="icon" src="../../static/3.png"></image>
			<text>搜美食,菜谱</text>
		</navigator>
	</view>
</template>

<script>
	import {baseUrl} from "@/api/apis.js"
	export default {
		data() {
			return {
				background: [],
				indicatorDots: true,  // 小点点
				autoplay: true,  // 自动播放
				interval: 5000,  // 播放间隔
				duration: 500  //  滚动时长
			}
		},
		created(){
			uni.request({
				url: baseUrl+"/index/home_banner",
				method: "POST",
				success: (res) => {
					this.background = res.data.data;
				}
			})
		}
	}
</script>

<style>
	.banner{
		width: 100%;
		height: 180px;
		position: relative;
	}
	.swiper,.img{
		width: 750rpx;
		height: 180px;
	}
	.search{
		width: 90%;
		height: 40px;
		border-radius: 20px;
		background: #fff;
		box-shadow: 1px 1px 15px 1px #ccc;
		position: absolute;
		left: 5%;
		bottom: -30px;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.icon{
		width: 15px;
		height: 15px;
		margin-right: 10px;
	}
</style>
