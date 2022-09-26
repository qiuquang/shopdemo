<template>
	<view>
		<Lines />
	</view>
</template>

<script>
	import Lines from '@/components/common/Lines.vue'
	export default {
		components: {Lines},
		data() {
			return {
				
			}
		},
		
		methods: {
			
		},
		onNavigationBarSearchInputClicked() {
			uni.navigateTo({
				url: "../search/search"
			})
		}
	}
</script>

<style>

</style>
