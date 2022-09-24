<template>
	<view class='commodity'>
		<!-- 单个商品组件 -->
		
		<view class='commodity-item' 
			v-for="(item,index) in dataList"
			:key='index'
		>
			<image class='commodity-img' :src="item.imgUrl" mode=""></image>
			<view class='commodity-content'>
				<text class='commodity-name'>{{item.name}}</text>
				<view>
					<text class='pprice'>¥{{item.pprice}}</text>
					<text class='oprice'>¥{{item.oprice}}</text>
				</view>
				<text class='discount'>{{item.discount}}折</text>
			</view>
		</view>
	
	</view>
</template>

<script>
export default {
	props:{
		dataList:Array
	}
}
</script>

<style scoped>
.commodity{
	display: flex;
	flex-wrap: wrap;
}
.commodity-item{
	width: 375rpx;
	padding-bottom:20rpx;
}
.commodity-img{
	width:100%;
	height: 375rpx;
}
.commodity-content{
	text-align: center;
}
.commodity-name{
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp:2;
	color:#333333;
	word-break: break-all;
	padding:6rpx 20rpx;
}
.oprice{
	text-decoration: line-through;
	font-size:24rpx;
	color:#999999;
}
.discount{
	border-radius: 4rpx;
	border:1px solid #FF3333;
	padding:2rpx 10rpx;
	font-size:20rpx;
	color:#FF3333;
}
</style>