<template>
	<view class="newsbox">
		<view class="pic">
			<image :src="item.picurl" mode="aspectFill"></image>
		</view>
		
		<view class="text">
			<view class="title">{{item.title}}</view>
			
			<view class="info" v-if="item.looktime">浏览时间：{{item.looktime}}</view>

				<view class="info" v-else>
					<text>作者:{{item.author}}</text>
					<text>{{item.hits}}浏览</text>
				</view>
				
				
			
		</view>
		
	</view>
</template>

<script>
	export default {
		name:"newsbox",
		props:{
			item:{
				type:Object,
				default(){
					return{
						title:"",
						author:"",
						picurl:"",
						hits:null,
						looktime:""
					}
				}
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.newsbox{
	display: flex;
	.pic{
		width: 230rpx;
		height: 160rpx;
		image{width: 100%;height: 100%;}
	}
	.text{
		flex: 1;
		padding-left: 20rpx;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		.title{
			font-size: 38rpx;
			color: #333;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			-webkit-line-clamp: 2;
			overflow: hidden;
		}
		.info{
			font-size: 26rpx;
			color: #999;
			text{
				padding-right: 30rpx;
			}
		}
	}
	
}
</style>