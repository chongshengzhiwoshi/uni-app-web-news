<template>
	<view class="details">
		<view class="title">{{details.title}}</view>
		
		<view class="info">
			<view class="author">编辑：{{details.author}}</view>
			<view class="time">发布时间：{{details.posttime}}</view>
		</view>
		
		<view class="content">
			<rich-text :nodes="details.content"></rich-text>
		</view>
	</view>
</template>

<script>
	import moment from "moment"
	import {parseTime} from "../../utils/tool.js"
	export default {
		data() {
			return {
				cid:0,
				id:0,
				details:{}
			};
		},
		onLoad(options) {
			const { id, cid } = options
			console.log('id:', id)
			console.log('cid:', cid)
			this.cid = cid
			this.id = id
			this.getDetails()
		},
		methods:{
			getDetails(){
				uni.showLoading({
					title: '加载中'
				});
				uni.request({
					url: `https://ku.qingnian8.com/dataApi/news/detail.php?cid=` + this.cid + '&id=' + this.id,
					success:res=>{
						this.details = res.data
						res.data.posttime = parseTime(this.details.posttime)
						this.setStorageCacke()
						uni.hideLoading()
					}
					
				})
				
			},
			setStorageCacke(){
				  var cackeArr = uni.getStorageSync('cacke_key') || [];
				  
				  var index = cackeArr.findIndex(item =>{
					  console.log("item.id="+item.id,"this.details.id="+this.details.id);
					  return item.id === this.details.id
				  })
				  console.log("index="+index);
				  let item = {
					  id:this.details.id,
					  cid:this.details.cid,
					  title:this.details.title,
					  picurl:this.details.picurl,
					  content:this.details.content,
					  looktime:parseTime(Date.now())
				  }
				  if(index != -1){
					  cackeArr.splice(index,1)
				  }
				  cackeArr.unshift(item);
				  uni.setStorageSync('cacke_key', cackeArr);
			}
		}
		
	}
</script>

<style lang="scss">
.details{
	padding: 30rpx;
	.title{
		font-size: 46rpx;
		color: #333;
	}
	.info{
		background: #F6F6F6;
		padding:20rpx 10rpx;
		font-size: 28rpx;
		color: #666;
		display: flex;
		justify-content: space-between;
		margin: 40rpx 0;
	}
	.content{
		padding-bottom: 50rpx;
		/deep/ img{
			max-width: 100%;
		}
	}
}
</style>
