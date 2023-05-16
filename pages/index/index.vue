<template>
	<view class="home">
			<scroll-view class="scroll" scroll-x>
				<view class="item" :class="index == navindex ? 'checkeditem' : ''" 
				v-for="(item,index) in titleArr" :key="item.id" @click="clickItem([index,item.id])">{{item.classname}}</view>
			</scroll-view>
			
			<view class="content" >
				<view class="row" v-for="item in listArr" @click="goDetails([item.classid,item.id])">
					<newsbox :item="{title:item.title,author:item.author,picurl:item.picurl,hits:item.hits}"></newsbox>
				</view>
			</view>
			
			<view class="loading" v-if="!listArr.length==0">
				<view v-if="loading==1">数据加载中...</view>
				<view v-if="loading==2">没有更多了~~</view>
			</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				titleArr:[],
				navindex:0,
				listArr:[],
				cid:0,
				currentPage:1,
				loading:0  //0:默认1:加载中2:没有更多了
			} 
		},
		onLoad() {
			this.getTitle()
			this.getList()
		},
		onReachBottom(){
			console.log("到底了");
			this.currentPage++;
			this.loading=1
			if(this.listArr.length==0){
				return;
			}
			this.getList()
		},
		methods: {
			goDetails([cid,id]){
				uni.navigateTo({
					url: `/pages/details/details?cid=`+cid+`&id=`+id
				})
				
				
			},
			getList(){
				
				uni.showLoading({
					title: '加载中'
				});
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
					data:{
						cid:this.cid,
						// num:6,
						page:this.currentPage
					},
					success:res=>{
						if(res.data.length==0){
						 	this.loading=2
						}
						this.listArr = [...this.listArr,...res.data]
						uni.hideLoading();
					}
					
				})
				
			},
			getTitle(){
				uni.showLoading({
					title: '加载中'
				});
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success:res=>{
						this.titleArr = res.data
						uni.hideLoading()
					}
				})
			},
			clickItem([index,itemId]){
				this.navindex = index
				this.cid = itemId
				this.currentPage=1
				this.listArr=[]
				this.getList()
			}
		}
		
	}
</script>

<style lang="scss" scoped>
.scroll {
	display: flex;
	white-space: nowrap; /* 让子元素不换行 */
    background-color: #ccc;
	
	position: fixed;
	top: var(--window-top);
	left: 0;
	z-index: 10;
	.item {
	  display: inline-flex;
	  justify-content: center;
	  align-items: center;
	  color: #000;
	  padding: 10px;
	  margin-right: 10px;
	  white-space: nowrap;
	  font-size: 40rpx;
	}
	.checkeditem{
		display: inline-flex;
		justify-content: center;
		align-items: center;
		background-color: #005500;
		color: #ffffff;
		padding: 10px;
		margin-right: 10px;
		white-space: nowrap;
	}
}
.content{
	padding: 30rpx;
	padding-top: 100rpx;
	.row{
		padding: 15rpx 0;
	}
}
.loading{
	text-align: center;
	font-size: 26rpx;
	color: #888;
	line-height: 2em;
}

</style>
