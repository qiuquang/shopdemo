<template>
	<view class='details'>
		<!--商品图-->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
			<swiper-item v-for='(item,index) in swiperList' :key='index'>
				<view class="swiper-item">
					<image class='swiper-img' :src="item.imgUrl" mode=""></image>
				</view>
			</swiper-item>
		</swiper>
		<!--价格和名称-->
		<view class='details-goods'>
			<view class='goods-pprice'>¥399.00</view>
			<view class='goods-oprice'>¥599.00</view>
			<view class='goods-name'>大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008大姨绒毛大款2020年必须</view>
		</view>
		<!--商品详情图-->
		<view>
			<view><image class='details-img' src="../../static/img/detail1.jpg" mode=""></image></view>
			<view><image class='details-img' src="../../static/img/detail2.jpg" mode=""></image></view>
			<view><image class='details-img' src="../../static/img/detail3.jpg" mode=""></image></view>
			<view><image class='details-img' src="../../static/img/detail4.jpg" mode=""></image></view>
			<view><image class='details-img' src="../../static/img/detail5.jpg" mode=""></image></view>
		</view>
		
		<Card cardTitle='看了又看'></Card>
		<CommodityList :dataList='dataList'></CommodityList>
		
	</view>
</template>

<script>
	import Card from '@/components/common/Card.vue'
	import CommodityList from '@/components/common/CommodityList.vue'
	export default {
		data() {
			return {
				swiperList:[
					{imgUrl:"../../static/img/details1.jpeg"},
					{imgUrl:"../../static/img/details2.jpeg"},
					{imgUrl:"../../static/img/details3.jpeg"}
				],
				dataList:[
					{
						id:1,
						imgUrl:"../../static/img/commodity1.jpg",
						name:"大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008",
						pprice:"299",
						oprice:"659",
						discount:"5.2"
					},
					{
						id:2,
						imgUrl:"../../static/img/commodity2.jpg",
						name:"大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008",
						pprice:"299",
						oprice:"659",
						discount:"5.2"
					},
					{
						id:3,
						imgUrl:"../../static/img/commodity3.jpg",
						name:"大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008",
						pprice:"299",
						oprice:"659",
						discount:"5.2"
					},
					{
						id:4,
						imgUrl:"../../static/img/commodity4.jpg",
						name:"大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008大姨绒毛大款2020年必须买,不买你就不行了,爆款疯狂GG008",
						pprice:"299",
						oprice:"659",
						discount:"5.2"
					}
				]
			}
		},
		components:{
			Card,
			CommodityList
		},
		methods: {
			
		}
	}
</script>

<style scoped>
swiper{
	width: 100%;
	height: 700rpx;
}
.swiper-img{
	width: 100%;
	height: 700rpx;
}
.details-goods{
	text-align: center;
	font-weight: bold;
	font-size:36rpx;
	padding:10rpx 0;
}
.details-img{
	width: 100%;
}
</style>
