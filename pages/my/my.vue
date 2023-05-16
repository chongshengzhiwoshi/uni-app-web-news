<template>
	<view class="user">
		<view class="top">
			<image src="../../static/images/历史.png"></image>
			<view class="text">浏览历史</view>
		</view>
		
		<view class="content">
			<view class="row" v-for="(item,index) in cackeArr" :key="item.id" @click="goDetails([item.classid,item.id])">
				<newsbox :item="{title:item.title,looktime:item.looktime,picurl:item.picurl}"></newsbox>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				details:{},
				cackeArr:[]
				// arr:[]
			};
		},
		onShow:function(){
			this.getCacke()
		},
		methods:{
			getCacke(){
				var cacke = uni.getStorageSync('cacke_key')
				this.cackeArr = cacke
				// if(!this.cackeArr.length == 0){
				// 	this.arr = this.cackeArr.slice().reverse()
				// }
				
				
			},
			goDetails([cid,id]){
				uni.navigateTo({
					url:`/pages/details/details?cid=`+cid+`&id=`+id
				})
			}

			
		}
	}
</script>

<style lang="scss">
.top{
	padding: 50rpx 0;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	image{
		width: 150rpx;
		height: 150rpx;
	}
	.text{
		font-size: 30rpx;
		padding-top: 20rpx;
	}
}
.content{
	padding: 30rpx;
	padding-top: 30rpx;
	.row{
		padding: 15rpx 0;
	}
}
</style>
