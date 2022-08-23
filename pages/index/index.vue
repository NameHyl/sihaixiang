<template>
	<view>
		<Banner />
		<Nav />
		<Line />
		<Mans />
		<Line />
		<Foods />
		<Line />
		<uni-rate v-model="rateValue" @change="onChange"/>
		<Footer />
	</view>
</template>

<script>
	import Banner from "./Banner.vue"
	import Nav from "./Nav.vue"
	import Mans from "./Mans.vue"
	import Foods from "./Foods.vue"
	import Footer from "./Footer.vue"
	import Line from "@/components/Line.vue"
	export default {
		data() {
			return {
				rateValue: 2
			}
		},
		components: {
			Banner,
			Nav,
			Mans,
			Foods,
			Footer,
			Line
		},
		onLoad() {

		},
		methods: {
		}
	}
</script>

<style>
</style>
