<template>
	<div class="banner-box">
		<div class="banner-view" :style="{width:width + 'px',height:height + 'px'}">
			<scroller class="banner" show-scrollbar="false" scroll-direction="horizontal" @scroll="handleScroll">
				<image class="image" v-for="(item,index) in items" :src="item.src" :style="{width:width + 'px',height:height + 'px'}" :ref="'image' + index"
				    @click="handleClick(index)"></image>
			</scroller>
			<div class="options" :style="{width:width + 'px'}">
				<div v-for="(item,index) in items" :class="['orb',index===currentIndex?'active':' ']" @click="handleOrb(index)"></div>
			</div>
		</div>
	</div>
</template>

<script>
	const dom = weex.requireModule('dom')
	const navigator = weex.requireModule('navigator')
	export default {
		name: "banner",
		props: {
			items: {
				type: Array,
				required: true,
				default(){
					return []
				}
			},
			width: {
				type: Number,
				default(){
					return 750
				}
			},
			height: {
				type: Number,
				default(){
					return 270
				}
			}
		},
		data() {
			return {
				currentIndex: 0,
				lastScroll: 0,
				anmFinish: true
			}
		},
		watch: {
			currentIndex(val, old) {
				if (val >= 0 && this.$data.anmFinish === true) {
					const el = this.$refs['image' + val][0]
					dom.scrollToElement(el, {})
					this.$data.anmFinish = false
					this.$emit("switch", this.items[val])
				}
				setTimeout(() => {
					this.$data.anmFinish = true
				}, 300)
			}
		},
		methods: {
			handleScroll(e) {
				let x = e.contentOffset.x
				let index = this.$data.currentIndex
				let abs = Math.abs(x - this.$data.lastScroll)
				if (abs > 100) {
					if (x < this.$data.lastScroll) {
						if (this.$data.anmFinish && this.items.length - 1 >= index + 1) this.$data.currentIndex = index + 1
					} else {
						if (this.$data.anmFinish && index - 1 >= 0) this.$data.currentIndex = index - 1
					}
					this.$data.lastScroll = x
				}
			},
			handleOrb(index) {
				if (index) {
					const el = this.$refs['image' + index][0]
					dom.scrollToElement(el, {})
					this.$data.currentIndex = index
				}
			},
			handleClick(index) {
				if(index >= 0){
					let current = this.items[index]
					if (current.url) {
						navigator.push({
							url: current.url,
							animated: "true"
						}, event => {
							this.$emit("tap", current)
						})
					} else {
						this.$emit("tap", current)
					}
				}
			}
		}
	}
</script>

<style scoped>
	.banner-box{
		width:750px;
		position: relative;
		justify-content: center;
		align-items: center;
	}
	
	.banner-view {
		width: 750px;
		position: relative;
		overflow: hidden;
	}

	.banner {
		flex-direction: row;
		position: relative;
	}

	.image {
		width: 750px;
	}

	.options {
		width: 750px;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		position: relative;
		bottom: 30px;
	}

	.orb {
		width: 14px;
		height: 14px;
		background-color: #999;
		opacity: 0.5;
		margin: 0 10px;
		border-radius: 50%;
	}

	.active {
		opacity: 1;
	}
</style>
