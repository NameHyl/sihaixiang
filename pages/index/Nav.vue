<template>
	<view class="nav">
		<view class="top">
			<view @click="go(i)" class="navItem" v-for="(item,i) in navArr" :key="i">
				<image class="icon" :src="item.img" mode=""></image>
				<view class="">
					{{ item.text }}
				</view>
			</view>
		</view>
		<navigator class="bottom">
			<image class="bigimg" src="../../static/8.png"></image>
		</navigator>
	</view>
</template>

<script>
	import local from "@/json/local.json"
	export default {
		data(){
			return{
				navArr:[]
			}
		},
		created() {
			// console.log(local);
			this.navArr = local.navlist
		},
		methods: {
			go(i){
				switch(i){
					case 0: uni.navigateTo({
						url: "/pages/car/car"
					});
					case 3: uni.navigateTo({
						url: "/pages/car/car"
					})
				}
			}
		}
	}
</script>

<style>
	.nav{
		width: 100%;
		height: auto;
	}
	.top{
		width: 100%;
		height: auto;
		display: flex;
		justify-content: space-around;
	}
	.navItem{
		text-align: center;
		padding: 40px 0px 10px 0px;
		box-sizing: border-box;
	}
	.icon{
		width: 60px;
		height: 60px;
	}
	.bigimg{
		width: 90%;
		height: 160px;
		display: block;
		margin: 15px auto;
	}
</style>
