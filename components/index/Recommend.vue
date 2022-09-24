<template>
	<view class='recommend bg-color'>
		<view v-for="(item,index) in dataList" :key="index" class='recommend-item'>
			<image class='item-big' :src="item.bigUrl" mode=""></image>
			<view class='item-small'>
				<image v-for="(itemm,indexx) in item.data" class='item-img' :src="itemm.imgUrl" mode=""></image>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		props: {
			dataList: Array
		}
	}
</script>

<style scoped>
.recommend{
	padding:20rpx;
}
.recommend-item{
	display: flex;
	flex-direction: column;
	border-radius: 20rpx;
	border: 2rpx solid #CCCCCC;
	overflow: hidden;
	margin:20rpx 0;
}
.item-big{
	width:100%;
	height: 300rpx;
}
.item-small{
	width:100%;
	height: 240rpx;
}
.item-img{
	width:33.3333%;
	height: 240rpx;
}
</style>
