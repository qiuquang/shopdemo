<template>
	<view class='index'>
		<!-- #ifdef MP-WEIXIN -->
		
		<view class="bar-height">
			
		</view>
		<!-- #endif -->
		<view class="bar1">
			234
		</view>
		<scroll-view scroll-x="true" class='scroll-content' :scroll-into-view='scrollIntoIndex'>
			<view
				:id="'top'+index"
				class='scroll-item'
				v-for='(item,index) in topBar'
				:key='index'
				@tap='changeTab(index)'
			>
				<text :class='topBarIndex===index? "f-active-color":"f-color"'>{{item.name}}</text>
			</view>
		</scroll-view>
		
		<swiper  @change='onChangeTab' :current="topBarIndex" :style="'height:'+clentHeight+'px;'">
			<swiper-item 
				v-for='(item,index) in newTopBar'
				:key='index'
			>
				<scroll-view scroll-y="true" :style="'height:'+clentHeight+'px;'"  @scrolltolower='loadMore(index)'>
					<template v-if="item.data.length>0"> 
						<block v-for="(itemm,indexx) in item.data" :key="indexx">
								<IndexSwiper v-if="itemm.type === 'swiperList'" :dataList="itemm.data"></IndexSwiper>
								<template v-if="itemm.type === 'recommendList'">
									<Recommend :dataList="itemm.data"></Recommend>
									<Card cardTitle='猜你喜欢'></Card>
								</template>
								
								<Banner v-if='itemm.type==="bannerList"' :dataList='itemm.imgUrl'></Banner>
															
								<template v-if='itemm.type==="iconsList"'>
									<Icons :dataList='itemm.data'></Icons>
									<Card cardTitle='热销爆品'></Card>
								</template>
								
								<template v-if='itemm.type==="hotList"'>
									<Hot :dataList='itemm.data'></Hot>
									<Card cardTitle='推荐店铺'></Card>
								</template>
								
								<template v-if='itemm.type==="shopList"'>
									<Shop :dataList='itemm.data'></Shop>
									<Card cardTitle='为您推荐'></Card>
								</template>
								
								<CommodityList v-if="itemm.type === 'commodityList'" :dataList="itemm.data"></CommodityList>
							</block>
					</template>
					<template v-else>
						<view>暂无数据...</view>
					</template>
					<view class='load-text f-color'>
						{{item.loadText}}
					</view>
				</scroll-view>
			</swiper-item>
		</swiper>
		
		
		<!-- 推荐模版 -->
		<!-- <IndexSwiper></IndexSwiper>
		<Recommend></Recommend>
		<Card cardTitle='猜你喜欢'></Card>
		<CommodityList></CommodityList> -->
		
		
		<!-- 其他模版：运动户外、美妆... -->
		<!-- <Banner></Banner>
		<Icons></Icons>
		<Card cardTitle='热销爆品'></Card>
		<Hot></Hot>
		<Card cardTitle='推荐店铺'></Card>
		<Shop></Shop>
		<Card cardTitle='为您推荐'></Card>
		<CommodityList></CommodityList> -->
		
	</view>
</template>

<script>
	import $http from '@/common/api/request.js'
	import IndexSwiper from '@/components/index/IndexSwiper.vue'
	import Recommend from '@/components/index/Recommend.vue'
	import Card from '@/components/common/Card.vue'
	import CommodityList from '@/components/common/CommodityList.vue'
	import Banner from '@/components/index/Banner.vue'
	import Icons from '@/components/index/Icons.vue'
	import Hot from '@/components/index/Hot.vue'
	import Shop from '@/components/index/Shop.vue'
	export default {
		data() {
			return {
				//选中的索引
				topBarIndex:0,
				//顶栏跟随的索引id值
				scrollIntoIndex:'top0',
				//内容块的高度值
				clentHeight:0,
				//顶栏数据
				topBar:[],
				// 承载数据
				newTopBar: []
			}
		},
		components:{
			IndexSwiper,
			Recommend,
			Card,
			CommodityList,
			Banner,
			Icons,
			Hot,
			Shop
		},
		onLoad() {
			this.init()
		},
		onReady() {
			// let view = uni.createSelectorQuery().select(".home-data");
			// view.boundingClientRect(data => {
			//     // this.clentHeight = data.height;
			//     this.clentHeight = 2000;
			// }).exec();
			uni.getSystemInfo({
				success: (res) => {
				  this.clentHeight = res.windowHeight - uni.upx2px(80);
				}
			})
		},
		//标题栏按钮点击
		onNavigationBarButtonTap(e){
			// console.log(e)
			if(e.float=='left'){
				uni.navigateTo({
					url:'../search/search'
				})
			}
		},
		methods:{
			init() {
				// uni.request({
				// 	url: 'http://192.168.0.100:3000/api/index_list/data',
				// 	success: (res) => {
				// 		const data = res.data.data;
				// 		this.topBar = data.topBar;
				// 		this.newTopBar = this.initData(data);						
				// 	}
				// })
				$http.request({
					url:"/index_list/data"
				}).then((res)=>{
					this.topBar = res.topBar;
					this.newTopBar = this.initData(res);
				}).catch(()=>{
					uni.showToast({
						title:'请求失败',
						icon:'none'
					})
				})
			},
			initData(res) {
				let arr = [];
				for(let i = 0; i < this.topBar.length; i++) {
					let obj = {
						data: [],
						load: 'first',
						loadText:"上拉加载更多..."
					}
					
					if(i === 0) {
						obj.data = res.data
					}
					
					arr.push(obj)
				}
				return arr;
			},
			changeTab(index){
				if(this.topBarIndex === index){
					return ;
				}
				this.topBarIndex = index;
				this.scrollIntoIndex = 'top'+index;
				
				if(this.newTopBar[index].load === 'first') {
					this.addData();
				}
				
			},
			onChangeTab(e){
				this.changeTab(e.detail.current);
			},
			//获取可视区域高度【兼容】
			getClientHeight(){
				const res = uni.getSystemInfoSync();
				const system = res.platform;
				if( system ==='ios' ){
					return 44+res.statusBarHeight;
				}else if( system==='android' ){
					return 48+res.statusBarHeight;
				}else{
					return 0;
				}
			},
			//对应显示不同数据
			addData(callback){
				//拿到索引
				let index = this.topBarIndex;
				//拿到id
				let id = this.topBar[index].id;
				
				let page = Math.ceil(this.newTopBar[index].data.length / 5) + 1
				// console.log(this.newTopBar[index].data.length,page)
				//请求数据
				$http.request({
					url:'/index_list/'+id+'/data/'+page+''
				}).then((res)=>{
					this.newTopBar[index].data = [...this.newTopBar[index].data,...res];
				}).catch(()=>{
					uni.showToast({
						title:'请求失败',
						icon:'none'
					})
				})
				
				//请求不同的数据
				// uni.request({
				// 	url:'http://192.168.0.100:3000/api/index_list/'+id+'/data/'+page,
				// 	success: (res) => {
				// 		if(res.statusCode !== 200) {
				// 			return
				// 		}
				// 		let data = res.data.data;
				// 		// console.log(data)
				// 		this.newTopBar[index].data = [...this.newTopBar[index].data,...data];
				// 	}
				// })
				
				//当请求结束后，重新赋值
				this.newTopBar[index].load='last';
				
				if(typeof callback==='function'){
					callback();
				}
			},
			//上拉加载更多
			loadMore(index){
				
				this.newTopBar[index].loadText = '加载中...';
				//请求完数据 ，文字提示信息又换成【上拉加载更多...】
				this.addData(()=>{
					this.newTopBar[index].loadText = '上拉加载更多...';
				})
			}
		}
	}
</script>

<style scoped>
.scroll-content{
	width: 100%;
	height: 80rpx;
	white-space: nowrap;
}
.scroll-item{
	display: inline-block;
	padding:10rpx 30rpx;
	font-size:32rpx;
}
.f-active-color{
	padding:10rpx 0;
	border-bottom:6rpx solid #49BDFB;
}
.load-text{
	border-top:2rpx solid #636263;
	line-height: 60rpx;
	text-align: center;
}
.bar-height{
	height: var(--status-bar-height);
}
.bar1{
	position: absolute;
	bottom: var(--window-bottom)
}
.bar2{
	height: var(--window-bottom);
	background-color: blue;
	width: 100%;
}

</style>
