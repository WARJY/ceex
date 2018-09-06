<template>
	<div>
		<div class="container" v-if="isShow">
			<div class="nav" ref="nav">
				<slot name="nav"></slot>
			</div>
			<div :style="{height:navHeight + 'px'}"></div>
			<scroller class="page" :style="{width:deviceWidth + 'px',minHeight:pageHeight + 'px'}" @scroll="handleScroll">
				<refresh class="refresh" :style="{width:deviceWidth + 'px'}" @refresh="handleRefresh" @pullingdown="handlePullDown" :display="refreshing ? 'show' : 'hide'">
					<div v-if="refreshSlot" ref="refreshSlot">
						<slot name="refreshSlot"></slot>
					</div>
					<loading-indicator v-if="!refreshSlot" class="indicator"></loading-indicator>
				</refresh>
				<div :style="{minHeight:(pageHeight + 1) + 'px'}">
					<slot></slot>
					<div :style="{height:bottomHeight + 'px'}"></div>
				</div>
				<loading class="loading" :style="{width:deviceWidth + 'px'}" @loading="handleLoading" :display="loading ? 'show' : 'hide'">
					<div v-if="loadSlot" ref="loadingSlot">
						<slot name="loadingSlot"></slot>
					</div>
					<loading-indicator v-if="!loadSlot" class="indicator-loading" :style="{bottom:bottomHeight + 'px'}"></loading-indicator>
				</loading>
			</scroller>
		</div>
		<div v-if="!isShow">
			<slot v-if="pageLoadSlot" name="pageLoading"></slot>
			<wxc-loading :show="!pageLoadSlot" type="default"></wxc-loading>
		</div>
	</div>
</template>

<script>
	import { WxcLoading } from 'weex-ui'
	const modal = weex.requireModule('modal')
	const dom = weex.requireModule('dom')
	export default {
		name: "Page",
		components:{
			WxcLoading:WxcLoading
		},
		props: {
			refreshing: {
				type: Boolean,
				default(){
					return false
				}
			},
			loading: {
				type: Boolean,
				default(){
					return false
				}
			},
			navbarHeight: {
				type: Number,
				default(){
					return 90
				}
			},
			isShow: {
				type: Boolean,
				required:true,
				default(){
					return false
				}
			},
			refreshSlot: {
				type: Boolean,
				default(){
					return false
				}
			},
			loadSlot: {
				type: Boolean,
				default(){
					return false
				}
			},
			pageLoadSlot:{
				type: Boolean,
				default(){
					return false
				}
			}
		},
		data() {
			return {
				deviceWidth: 750,
				deviceHeight: 0,
				pageHeight: 0,
				navHeight: 0,
				bottomHeight: 0,
				firstLoad: false
			}
		},
		created() {
			this.$data.deviceHeight = WXEnvironment.deviceHeight
			this.$data.bottomHeight = this.navbarHeight
		},
		watch: {
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
								console.log("e")
							}
						})
						let process = [getNav]
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
			handleScroll(e){
				this.$emit("scroll",e)
			},
			handlePullDown(e){
				this.$emit("pullDown",e)
			},
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
	.container {
		position: relative;
	}

	.page {
		background-color: #f5f5f5;
	}

	.nav {
		position: absolute;
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

	.indicator-loading {
		height: 40px;
		width: 40px;
		color: #00d2ff;
		margin-top: 16px;
		margin-bottom: 16px;
	}
</style>
