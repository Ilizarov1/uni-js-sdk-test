<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<input type="text" v-model="bucketName" placeholder="请先输入空间名" />
		<input type="text" v-model="secretKey" placeholder="请先输入secretKey" />
		<input type="text" v-model="accessKey" placeholder="请先输入accessKey" />
		<button size="mini" :disabled="false" hover-start-time=20 @click="handleClick">选择文件</button>
	</view>
</template>
<script>
	import TokenTools from '../../utlis/token'
	import * as qiniu from 'qiniu-js'
	function handleUpload(file, accessKey, secretKey, scope) {
				const observer = {
					next(res){},
					error(res){
						console.log(res);
					},
					complete(res){
						alert('上传成功',res)
					}
				}
				const token = TokenTools.genUpToken(accessKey, secretKey,{
					scope,
					deadline: Math.floor(Date.now() / 1000) + 3600
					})
				const observable = qiniu.upload(file, null, token)
				const subscription = observable.subscribe(observer)

			}
	export default {
		data() {
			return {
				title: 'Hello',
				accessKey: '',
				secretKey: '',
				bucketName: ''
			}
		},
		onLoad() {
			
		},
		methods: {
			handleClick() {
				const ak = this.accessKey
				const sk = this.secretKey
				const bucketName = this.bucketName
				// 选择图片文件
				uni.chooseFile({
					count: 1,
					type: 'image',
					success (res) {
						const tempFilePaths = res.tempFiles
						handleUpload(tempFilePaths[0], ak, sk, bucketName)
					}
				})
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin: 200rpx auto 50rpx auto;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
