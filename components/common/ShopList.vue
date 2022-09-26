<template>
	<view class='shop-list'>
		
		<view class='shop-title f-color'>
			<view class='shop-item'
				v-for="(item,index) in shopList.data"
				:key='index'
				@tap='changTab(index)'
			>
				<view :class="  shopList.currentIndex==index?'f-active-color':'' ">{{item.name}}</view>
				<view class='shop-icon'>
					<view class='iconfont icon-xiangshangjiantou up'
						:class=' item.status ===1 ? "f-active-color":"" '
					></view>
					<view class='iconfont icon-xiangxiajiantou down'
						:class=' item.status ===2 ? "f-active-color":"" '
					></view>
				</view>
			</view>
		</view>
		<Lines />
		<CommodityList :dataList='dataList'></CommodityList>
	</view>
</template>

<script>
import $http from '@/common/api/request.js'
import Lines from '@/components/common/Lines.vue'
import CommodityList from './CommodityList.vue'
export default {
		data() {
			return {
				shopList:{
				    currentIndex:0,
					data:[
						{name:"价格",status:1, key: 'pprice'},
						{name:"折扣",status:0, key: 'discount'},
						{name:"品牌",status:0}
					]
				},
				dataList:[]
			}
		},
		computed: {
			orderBy() {
				let obj = this.shopList.data[this.shopList.currentIndex];
				let val = obj.status === '1' ? 'desc' : 'asc';
				return {
					[obj.key]: val
				}
			}
		},
		props: {
			keyword: {
				type:String
			}
		},
		components:{
			Lines,
			CommodityList
		},
		mounted() {
			this.getData();
		},
		methods: {
			getData() {
				// console.log(this.keyword)
				$http.request({
					url:"/goods/search",
					data: {
						name: this.keyword,
						...this.orderBy
						// pprice: "desc"
					}
				}).then((res)=>{
					// console.log(res)
					this.dataList = res;
					// this.topBar = res.topBar;
					// this.newTopBar = this.initData(res);
				}).catch(()=>{
					uni.showToast({
						title:'请求失败',
						icon:'none'
					})
				})
			},
			changTab(index){
				this.getData()
				//索引值
				let idx = this.shopList.currentIndex;
				//具体哪一个对象
				let item = this.shopList.data[idx];
				if( idx == index ){
					return item.status = item.status === 1 ? 2:1;
				}
				//新的值
				let newItem = this.shopList.data[index];
				item.status = 0;
				this.shopList.currentIndex = index;
				newItem.status = 1;
			}
		}
	}
</script>

<style scoped>
.shop-title{
	display: flex;
}
.shop-item{
	flex:1;
	display: flex;
	justify-content: center;
	align-items: center;
	height: 80rpx;
}
.shop-icon{
	position: relative;
	margin-lef:10rpx;
}
.iconfont{
	width:16rpx;
	height: 8rpx;
	position: absolute;
	left:0;
}
.up{
	top:-34rpx;
}
.down{
	top:-24rpx;
}
</style>
