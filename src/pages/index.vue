<template>
	<NavMain :iconfont="iconfont" :setting="navSetting" :styleDefault="styleDefault" :styleActive="styleActive" @switch="handleSwitch">
		<div slot="index">
			<Page :isShow="showindex" :refreshing="refreshing" @refreshing="handleRefresh" :loading="loading" @loadining="handleLoading" :navbarHeight="90">
				<wxc-minibar slot="nav" title="标题" background-color="#009ff0" text-color="#FFFFFF" right-text="更多"></wxc-minibar>
				<Banner :setting="bannerSetting" />
			</Page>
		</div>
		<Page slot="my" :isShow="showmy" :refreshing="refreshing" @refreshing="handleRefresh" :loading="loading" @loadining="handleLoading" :navbarHeight="90">
			<Banner :setting="bannerSetting" />
		</Page>
	</NavMain>
</template>

<script>
	import Page from "@/components/Page"
	import NavMain from "@/components/NavMain"
	import Banner from "@/components/Banner"
	import {
		WxcMinibar
	} from 'weex-ui'
	export default {
		name: "index",
		components: {
			Page: Page,
			NavMain: NavMain,
			Banner: Banner,
			WxcMinibar: WxcMinibar
		},
		data: function () {
			return {
				refreshing: false,
				loading: false,
				showindex: false,
				showmy: false,
				iconfont: "//at.alicdn.com/t/font_811848_qrm8hrhlfkg.ttf",
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
// 				navSetting: [{
// 						icon: "https://vuejs.org/images/logo.p ng",
// 						activeIcon: " ",
// 						title: "首页"
// 					},
// 					{
// 						icon: "https://vuejs.org/images/logo.png",
// 						activeIcon: " ",
// 						title: "首页"
// 					}
// 				],
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
						src: "https://vuejs.org/images/logo.png"
					},
					{
						src: "https://vuejs.org/images/logo.png"
					},
					{
						src: "https://vuejs.org/images/logo.png"
					}
				]
			}
		},
		mounted: function () {
			
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
			handleSwitch: function(e){
				if(e.load){
					this.$data.showLoading = true
					setTimeout(()=>{
						for(let i in e.switch){
							this.$data["show" + i] = e.switch[i]
							this.$forceUpdate()
						}
						this.$data.showLoading = false
					},1000)
				}
			}
		}
	}
</script>

<style scoped>

</style>
