<template>
	<div>
		<wxc-minibar title="标题" background-color="#009ff0" text-color="#FFFFFF" right-text="更多" />
		<tab-bar :iconfont="iconfont" :items="navSetting" :index="0" :styleDefault="styleDefault" :styleActive="styleActive" @switch="handleSwitch">
			<page slot="index" :isShow="showindex" :refreshing="refreshing" @refreshing="handleRefresh" :loading="loading" @loadining="handleLoading" :toTop="true">
				<banner :items="bannerSetting" />
				<menu :row="4" :items="menuSetting" />
				<listc title="我是标题" :items="listItems"></Listc>
				<listc type="custom" title="我是custom" :items="listItems2"></Listc>
			</page>
			<page slot="my" :isShow="showmy" :refreshing="refreshing" @refreshing="handleRefresh" :loading="loading" @loadining="handleLoading">
			</page>
		</tab-bar>
	</div>
</template>

<script>
	import Page from "@/components/Page/index.js"
	import TabBar from "@/components/TabBar/index.js"
	import Banner from "@/components/Banner/index.js"
	import Menu from "@/components/Menu/index.js"
	import Listc from "@/components/List/index.js"
	import {
		WxcMinibar
} from 'weex-ui';
	export default {
		name: "index",
		components: {
			Page,
			TabBar,
			Banner,
			WxcMinibar,
			Menu,
			Listc
		},
		data() {
			return {
				refreshing: false,
				loading: false,
				showindex: false,
				showmy: false,
				iconfont: "//at.alicdn.com/t/font_811848_8vtd3k91r3i.ttf",
				navSetting: [
					{
						name: "index",
						icon: "\ue751;",
						title: "首页"
					},
					{
						name: "my",
						icon: "\ue75e;",
						title: "我的"
					}
				],
				navSetting2: [
					{
						icon: "https://vuejs.org/images/logo.p ng",
						activeIcon: " ",
						title: "首页"
					},
					{
						icon: "https://vuejs.org/images/logo.png",
						activeIcon: " ",
						title: "首页"
					}
				],
				styleDefault: {
					icon: {
						color: '#333'
					},
					title: {
						color: '#333'
					}
				},
				styleActive: {
					icon: {
						color: '#00d2ff'
					},
					title: {
						color: '#00d2ff'
					}
				},
				bannerSetting: [
					{
						src: "https://vuejs.org/images/logo.png",
						url: ""
					},
					{
						src: "https://vuejs.org/images/logo.png"
					},
					{
						src: "https://vuejs.org/images/logo.png"
					}
				],
				menuSetting:[
					{
						src: "https://vuejs.org/images/logo.png",
						title: "午餐",
						url: " "
					},
					{
						src: "https://vuejs.org/images/logo.png",
						title: "晚餐",
						url: " "
					},
					{
						src: "https://vuejs.org/images/logo.png",
						title: "晚餐",
						url: " "
					},
					{
						src: "https://vuejs.org/images/logo.png",
						title: "晚餐",
						url: " "
					}
				],
				listItems:[
					{src:"https://vuejs.org/images/logo.png",title:"设置"},
					{src:"https://vuejs.org/images/logo.png",title:"设置"}
				],
				listItems2:[
					{src:"https://vuejs.org/images/logo.png",h1:"设置",h2:"我是设置",h3:"我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内",h1side:"h1side",h2side:"h2side",h3side:"h3side",label:["label1","label2"]},
				]
			}
		},
		methods: {
			handleRefresh: function () {
				this.$data.refreshing = true
				setTimeout(() => {
					this.$data.refreshing = false
				}, 1000)
			},
			handleLoading: function () {
				this.$data.loading = true
				setTimeout(() => {
					this.$data.loading = false
				}, 1000)
			},
			handleSwitch: function (e) {
				console.log(e)
				if (e.load) {
					setTimeout(() => {
						this.$data["show" + e.load.name] = true
					}, 1000)
				}
			}
		}
	}
</script>

<style scoped>

</style>
