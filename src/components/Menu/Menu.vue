<template>
	<div class="menu">
		<div v-for="(item,index) in items" class="menu-item" :style="{width:itemWidth}" @click="handleTap(index)">
			<image class="icon" :style="styleDefault.icon" :src="item.src" />
			<text class="title" :style="styleDefault.title">{{item.title}}</text>
		</div>
	</div>
</template>

<script>
	const navigator = weex.requireModule('navigator')
	export default {
		name: "Menu",
		props: {
			row: {
				type: Number,
				required: true,
				default () {
					return 5
				}
			},
			items: {
				type: Array,
				required: true,
				default () {
					return []
				}
			},
			styleDefault:{
				type:Object,
				default(){
					return {
						icon:{},
						title:{}
					}
				}
			}
		},
		computed: {
			itemWidth() {
				return 750 / this.row + 'px'
			}
		},
		methods: {
			handleTap(index) {
				if (index >= 0) {
					let current = this.items[index]
					if (current.url) {
						navigator.push({
							url: current.url,
							animated: "true"
						}, event => {
							this.$emit("tap",current)
						})
					}else{
						this.$emit("tap",current)
					}
				}
			}
		}
	}
</script>

<style scoped>
	.menu {
		width: 750px;
		flex-direction: row;
		flex-wrap: wrap;
		align-items: center;
		justify-content: flex-start;
		padding-top: 20px;
		padding-bottom: 20px;
		background-color: #fff;
	}

	.menu-item {
		position: relative;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	.icon {
		width: 60px;
		height: 60px;
	}

	.title {
		font-size: 24px;
		margin-top: 10px;
		color: #333;
	}
</style>
