<template>
	<div>
		<div v-if="isShow">
			<div class="nav" ref="nav">
				<slot name="nav"></slot>
			</div>
			<div :style="{height:navHeight + 'px'}"></div>
			<scroller class="page" :style="{width:deviceWidth + 'px',minHeight:pageHeight + 'px'}" @loadmore="handleLoading">
				<refresh class="refresh" :style="{width:deviceWidth + 'px'}" @refresh="handleRefresh" :display="refreshing ? 'show' : 'hide'">
					<div ref="refreshSlot">
						<slot name="refreshSlot"></slot>
					</div>
					<loading-indicator class="indicator" v-if="!refreshSlot"></loading-indicator>
				</refresh>
				<div :style="{minHeight:(pageHeight + 1) + 'px'}">
					<slot></slot>
					<div :style="{height:bottomHeight + 'px'}"></div>
				</div>
				<loading class="loading" :style="{width:deviceWidth + 'px'}" @loading="handleLoading" :display="loading ? 'show' : 'hide'">
					<div ref="loadingSlot">
						<slot name="loadingSlot"></slot>
					</div>
					<loading-indicator class="indicator-loading" :style="{bottom:bottomHeight + 'px'}" v-if="!loadingSlot"></loading-indicator>
				</loading>
			</scroller>
		</div>
		<wxc-loading :show="!isShow" type="default"></wxc-loading>
	</div>
</template>

<script>
	import {
		WxcLoading
	} from 'weex-ui';
	const modal = weex.requireModule('modal')
	const dom = weex.requireModule('dom')
	export default {
		name: "page",
		props: {
			refreshing: {
				type: Boolean,
				default: false
			},
			loading: {
				type: Boolean,
				default: false
			},
			navbarHeight: {
				type: Number,
				default: 90
			},
			isShow: {
				type: Boolean,
				default: false
			}
		},
		components: {
			WxcLoading: WxcLoading
		},
		data() {
			return {
				deviceWidth: 750,
				deviceHeight: 0,
				pageHeight: 0,
				navHeight: 0,
				bottomHeight: 0,
				refreshSlot: false,
				loadingSlot: false,
				firstLoad: false
			}
		},
		created() {
			this.$data.deviceHeight = WXEnvironment.deviceHeight
			this.$data.bottomHeight = this.navbarHeight
		},
		watch: {
			loading:function(val,old){
			},
			isShow: function (val, old) {
				if (val === true && this.$data.firstLoad === false) {
					setTimeout(() => {
						let getNav = new Promise((r, j) => {
							try {
								dom.getComponentRect(this.$refs.nav, option => {
									if (option.result) {
										let navHeight = option.size.height
										if (navHeight > 0) this.$data.navHeight = navHeight
										this.$data.pageHeight = this.$data.deviceHeight - navHeight - this.navbarHeight - 40
										r(true)
									}
								})
							} catch (e) {
								j(false)
							}
						})
						let getRefreshSlot = new Promise((r, j) => {
							try {
								dom.getComponentRect(this.$refs.refreshSlot, option => {
									if (option.result === true) this.$data.refreshSlot = true
									r(true)
								})
							} catch (e) {
								j(false)
							}
						})
						let getLoadingSlot = new Promise((r, j) => {
							try {
								dom.getComponentRect(this.$refs.loadingSlot, option => {
									if (option.result === true) this.$data.loadingSlot = true
									r(true)
								})
							} catch (e) {
								j(false)
							}
						})
						let process = [getNav, getRefreshSlot, getLoadingSlot]
						Promise.all(process).then(data => {
							let flag = true
							for (let i in data) {
								if (data[i] !== true) flag = false
							}
							if (flag === true) this.$data.firstLoad = true
						})
					}, 100)
				}
			}
		},
		methods: {
			handleRefresh(e) {
				this.$emit("refreshing")
			},
			handleLoading(e) {
				this.$emit("loadining")
			}
		}
	}
</script>

<style scoped>
	.page {
		background-color: #f5f5f5;
	}

	.nav {
		position: fixed;
		top: 0;
		z-index: 100;
	}

	.refresh {
		text-align: center;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		padding-top: 16px;
		padding-bottom: 16px;
	}

	.loading {
		text-align: center;
		position: relative;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}

	.indicator {
		height: 40px;
		width: 40px;
		color: #00d2ff;
	}
	
	.indicator-loading{
		height: 40px;
		width: 40px;
		color: #00d2ff;
		margin-top: 16px;
		margin-bottom: 16px;
	}
</style>
