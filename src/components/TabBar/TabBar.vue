<template>
	<div>
		<div class="nav-content" :style="{minHeight:deviceHeight + 'px'}">
			<div class="nav-container" ref="nav" :style="{width:750*setting.length + 'px',height:deviceHeight + 'px',transform:`translateX(${tranlateX})`}">
				<div v-for="(item,index) in setting">
					<div class="nav-tab" :style="{minHeight:deviceHeight + 'px'}">
						<slot :name="item.name"></slot>
					</div>
				</div>
			</div>
		</div>
		<div class="nav-main">
			<div v-for="(item,index) in setting" class="nav-item" v-on:click="handleSwitch(index)">
				<!--字体图标-->
				<div v-if="iconfont">
					<text :class="['icon',index===currentIndex?'active':'']" :style="[index===currentIndex?styleActive.icon:styleDefault.icon]">{{item.icon}}</text>
					<text :class="['title',index===currentIndex?'active':'']" :style="[index===currentIndex?styleActive.title:styleDefault.title]">{{item.title}}</text>
				</div>
				<!--图片图标-->
				<div v-if="!iconfont">
					<image class="icon" :src="index===currentIndex?item.activeIcon:item.icon" :style="[index===currentIndex?styleActive.icon:styleDefault.icon]"></image>
					<text :class="['title',index===currentIndex?'active':'']" :style="[index===currentIndex?styleActive.title:styleDefault.title]">{{item.title}}</text>
				</div>
				<div class="nav-slot">
					<slot :name="item.name + 'Bar'"></slot>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	const domModule = weex.requireModule('dom')
	const animation = weex.requireModule('animation')
	export default {
		name: "TabBar",
		props: {
			setting: {
				type: Array,
				required: true,
				default(){
					return []
				}
			},
			index: Number,
			duration: {
				type: Number,
				default(){
					return 0
				}
			},
			iconfont: String,
			styleDefault: {
				type: Object,
				default: function () {
					return {
						icon: {},
						title: {}
					}
				}
			},
			styleActive: {
				type: Object,
				default: function () {
					return {
						icon: {},
						title: {}
					}
				}
			}
		},
		data() {
			return {
				currentIndex: 0,
				deviceHeight: 0,
				hasLoad: {},
				tranlateX: 0
			}
		},
		watch: {
			currentIndex(val, old) {
				if (val >= 0) {
					let nav = this.$refs.nav
					if (this.duration > 0) {
						animation.transition(nav, {
							styles: {
								transform: `translateX(${-750*val}px)`,
							},
							duration: this.duration
						})
					} else {
						this.$data.tranlateX = -750 * val + 'px'
					}
				}
			},
			index(val,old){
				if(val !== old){
					this.$data.currentIndex = val
				}
			}
		},
		created() {
			if (this.iconfont) {
				domModule.addRule('fontFace', {
					'fontFamily': "iconfont",
					'src': "url('" + this.iconfont + "')"
				})
			}
			if (this.index) this.$data.currentIndex = this.index
			this.$data.deviceHeight = WXEnvironment.deviceHeight
			let hasLoad = Object.create(null)
			let setting = this.setting
			for (let k in setting) {
				hasLoad[setting[k].name] = parseInt(k) !== this.$data.currentIndex ? false : true
			}
			this.$data.hasLoad = hasLoad
			this.$emit("switch", {
				load: this.setting[this.$data.currentIndex],
				target: this.setting[this.$data.currentIndex]
			})
		},
		methods: {
			handleSwitch(index) {
				let name = this.setting[index].name
				if (index >= 0 && this.$data.currentIndex >= 0) {
					if (index !== this.$data.currentIndex) {
						this.$data.currentIndex = index
					}
				}
				if (this.$data.hasLoad[name] === false) {
					this.$data.hasLoad[name] = true
					this.$emit("switch", {
						load: this.setting[this.$data.currentIndex],
						target: this.setting[this.$data.currentIndex]
					})
					return
				} else {
					this.$emit("switch", {
						target: this.setting[this.$data.currentIndex]
					})
				}
			}
		}
	}
</script>

<style scoped>
	.nav-content {
		width: 750px;
		background-color: #F5F5F5;
		overflow: hidden;
	}

	.nav-container {
		align-items: center;
		justify-content: center;
		flex-direction: row;
		position: relative;
	}

	.nav-main {
		width: 750px;
		height: 98px;
		background-color: #fff;
		border-top-width: 1px;
		border-top-color: #999;
		position: fixed;
		bottom: 0;
		z-index: 1000;
		align-items: center;
		justify-content: center;
		flex-direction: row;
	}

	.nav-tab {
		width: 750px;
	}

	.nav-placeholer {
		height: 98px;
	}

	.nav-item {
		align-items: center;
		justify-content: center;
		flex: 1;
		position: relative;
	}
	
	.nav-slot{
		position: absolute;
		top: 0;
		left: 0;
		bottom: 0;
		right: 0;
		align-items: center;
		justify-content: center;
	}

	.icon {
		font-family: iconfont;
		font-size: 44px;
		width: 44px;
		height: 44px;
	}

	.title {
		font-size: 20px;
	}
</style>
