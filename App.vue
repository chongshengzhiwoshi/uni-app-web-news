<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
.view{
	box-sizing: border-box;
}
.content{
	padding: 30rpx;
	.row{
		padding: 15rpx 0;
	}
}
</style>
