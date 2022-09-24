<template>
	<view class="index">
		<IndexSwiper></IndexSwiper>
		<Recommend></Recommend>
		<view class="f-active-color">
			文字
		</view>
		<view class="iconfont icon-xiaoxi">
			消息
		</view>
	</view>
</template>

<script>
	import IndexSwiper from '@/components/index/IndexSwiper.vue'
	import Recommend from '@/components/index/Recommend.vue'
	export default {
		components: {IndexSwiper, Recommend},
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
</style>
