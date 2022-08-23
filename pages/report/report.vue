<template>
	<view class="report">
		<view class="typeBox">
			<view @click="selTag(i)" :class="item.ischecked?'typeItem cur':'typeItem'" v-for="(item,i) in typeArr"
				:key="i">
				{{ item.title }}
			</view>
		</view>
		<view class="areaBox">
			<textarea v-model="area" placeholder="请输入您的意见" />
		</view>
		<view class="upImgBox">
			<image v-if="show" class="imgs" :src="upimgs"></image>
			<view class="upBtn" @click="upimg">
				<text class="jia">+</text>
				<view class="text">最多上传9张</view>
			</view>
		</view>
		<view class="btn" @click="up">
			确认提交
		</view>
	</view>
</template>

<script>
	import {baseUrl,imgUrl} from "@/api/apis.js"
	export default {
		data() {
			return {
				sindex: -1,
				area: "",
				typeArr: [{
						title: "菜品问题",
						ischecked: false
					},
					{
						title: "服务问题",
						ischecked: false
					},
					{
						title: "支付问题",
						ischecked: false
					},
				],
				selArr: [], // 选中投诉类型的数组
				show: false,// 控制图片显示或者隐藏
				upimgs: ""  // 图片要显示的路径
			}
		},
		methods: {
			selTag(i) {
				// this.sindex = i; 单选
				this.typeArr[i].ischecked = !this.typeArr[i].ischecked;
				if (!this.selArr.includes(i)) {
					this.selArr.push(i); // 如果数组中不包含传入的索引,就往新数组中添加数据
				} else {
					// 如果点击的时候,传入的索引. 过滤新数组中,和传入索引相同的数据(如果相同 就应该删掉该索引数据)
					this.selArr = this.selArr.filter(item => item != i)
				}
				console.log(this.selArr);
			},
			// 提交事件
			up() {
				if( this.selArr.length == 0 ){
					uni.showToast({
						title: "未选择类型",
						icon: "loading",
						duration: 2000
					})
				}else if(this.area == ""){
					uni.showToast({
						title: "请输入内容",
						icon: "loading",
						duration: 2000
					})
				}else if(this.upimgs == ""){
					uni.showToast({
						title: "未上传图片",
						icon: "loading",
						duration: 2000
					})
				}else{
					// 发请求 提交给后台
					uni.showToast({
						title: "提交成功",
						duration: 2000
					})
				}
			},
			// 点击提交上传图片
			upimg() {
				let _this = this;
				uni.chooseImage({
					count: 9, //默认9 上传图片最大的张数 
					sizeType: ['original', 'compressed'], //可以指定是original原图还是compressed压缩图，默认二者都有
					sourceType: ['album', 'camera'], //从album相册选择  camera相机拍照
					success: function(res) {
						let tempFilePaths = res.tempFilePaths; // 临时路径的数组
						// 拿到tempFilePaths 它是一个数组,循环遍历 循环提交上传图片的接口,最终把resimg push到数组中
						//  视图上做循环遍历.
						uni.uploadFile({
							url: baseUrl+'/plugs/uploads', //仅为示例，非真实的接口地址
							filePath: tempFilePaths[0],  //  临时路径
							name: 'files',
							formData: {},
							success: (res) => {
								let resimg = JSON.parse(res.data);
								_this.upimgs = imgUrl + resimg.data;
								// 可以显示图片
								_this.show = true
							}
						});
					}
				});
			}
		}
	}
</script>

<style>
	page {
		width: 100%;
		height: 100%;
	}

	.report {
		width: 100%;
		height: 100%;
		position: relative;
	}

	.typeBox {
		width: 90%;
		height: auto;
		margin: auto;
		display: flex;
		flex-wrap: wrap;
	}

	.typeItem {
		width: auto;
		padding: 5px 10px;
		margin: 7px;
		box-sizing: border-box;
		border: 1px solid #ccc;
		color: #ccc;
		border-radius: 20px;
	}

	.cur {
		background: orange;
		color: #fff;
		border: 1px solid orange;
	}

	.areaBox {
		width: 90%;
		margin: auto;
	}

	.upImgBox {
		width: 90%;
		margin: auto;
		display: flex;
	}

	.imgs {
		width: 80px;
		height: 80px;
	}

	.upBtn {
		width: 80px;
		height: 80px;
		background: #ccc;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	.jia {

		font-size: 36px;
	}

	.text {
		font-size: 12px;
	}

	.btn {
		width: 100%;
		height: 40px;
		text-align: center;
		line-height: 40px;
		background: orange;
		color: #fff;
		position: fixed;
		bottom: 0;
	}
</style>
