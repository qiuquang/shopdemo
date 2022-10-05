<template>
	<view class='commodity' :style="{flexWrap: wrap}">
		<!-- 单个商品组件 -->
		
		<view class='commodity-item' 
			v-for="(item,index) in dataList"
			:key='index'
			:style="{width: itemW}"
			@tap='goDetails'
		>
			<image class='commodity-img' :style="{height: bigH}" :src="item.imgUrl" mode=""></image>
			<view class='commodity-content'>
				<text :style="{fontSize: nameSize}" class='commodity-name'>{{item.name}}</text>
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
		dataList:Array,
		itemW: {
			type: String,
			default: '375rpx'
		},
		bigH: {
			type: String,
			default: '375rpx'
		},
		wrap: {
			type: String,
			default: 'wrap'
		},
		nameSize: {
			type: String,
			default: '26rpx'
		}
	},
	methods: {
		goDetails(){
			uni.navigateTo({
				url:'../../pages/details/details'
			})
		}
	}
}
</script>

<style scoped>
.commodity{
	display: flex;
}
.commodity-item{
	padding-bottom:20rpx;
}
.commodity-img{
	width:100%;
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