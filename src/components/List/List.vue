<template>
	<div class="list">
		<text v-if="title" class="list-title">{{title}}</text>
		<div class="list-content">
			<div v-if="type==='default'" v-for="(item,index) in items" class="list-default" :style="childStyle(index)" @click="handleTap(index)">
				<image class="list-default-img" :src="item.src" :style="defaultStyle.img"></image>
				<div class="list-default-bd" :style="defaultStyle.title">{{item.title}}</div>
				<text class="list-default-ft" :style="defaultStyle.arrow">&#xe775;</text>
			</div>
			<div v-if="type==='custom'" v-for="(item,index) in items" class="list-custom" :style="childStyle(index)" @click="handleTap(index)">
				<div class="list-custom-main">
					<image v-if="item.src" class="list-custom-img" :src="item.src"></image>
					<div class="list-custom-content">
						<div class="list-custom-h1">
							<text v-if="item.h1" class="list-custom-h1-content" :style="customStyle.h1">{{item.h1}}</text>
							<text v-if="item.h1side" class="list-custom-h1-side" :style="customStyle.h1side">{{item.h1side}}</text>
						</div>
						<div class="list-custom-h2">
							<text v-if="item.h2" class="list-custom-h2-content" :style="customStyle.h2">{{item.h2}}</text>
							<text v-if="item.h2side" class="list-custom-h2-side"  :style="customStyle.h2side">{{item.h2side}}</text>
						</div>
						<div class="list-custom-h3">
							<text v-if="item.h3" class="list-custom-h3-content" :style="customStyle.h3">{{item.h3}}</text>
							<text v-if="item.h3side" class="list-custom-h3-side" :style="customStyle.h3side">{{item.h3side}}</text>
						</div>
					</div>
				</div>
				<div v-if="item.label" class="list-custom-plus">
					<div class="list-custom-img" style="height: 0px;"></div>
					<div class="list-custom-label">
						<text v-for="label in item.label" class="list-custom-label-item" :style="customStyle.label">{{label}}</text>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	const dom = weex.requireModule('dom')
	const navigator = weex.requireModule('navigator')
	export default {
		name: "List",
		props: {
			type:{
				type:String,
				default(){
					return "default"
				}
			},
			title:{
				type:String,
				default(){
					return ""
				}
			},
			items:{
				type:Array,
				required:true,
				default(){
					return []
				}
			},
			defaultStyle:{
				type:Object,
				default(){
					return {
						img:{},
						title:{},
						arrow:{}
					}
				}
			},
			customStyle:{
				type:Object,
				default(){
					return {
						img:{},
						h1:{},
						h1side:{},
						h2:{},
						h2side:{},
						h3:{},
						h3side:{},
						label:{}
					}
				}
			}
		},
		data() {
			return {
				childStyle(index){
					if(index === this.items.length-1) return {borderBottomWidth:0}
				}
			}
		},
		created() {
			dom.addRule('fontFace', {
				'fontFamily': "iconfont",
				'src': "url('http://at.alicdn.com/t/font_811848_8vtd3k91r3i.ttf')"
			});
		},
		methods: {
			handleTap(index){
				let current = this.items[index]
				if(current.url){
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
</script>

<style scoped>
	.list {
		width: 750px;
		background-color: #fff;
		border-top-width: 1px;
		border-bottom-width: 1px;
		border-color: #d9d9d9;
	}

	.list-title {
		width: 750px;
		font-size: 30px;
		padding: 20px;
		color: #666;
		font-weight: bold;
		border-bottom-width: 1px;
		border-color: #d9d9d9;
	}
	
	.list-content{
		position: relative;
		flex-direction: column;
		align-items: flex-start;
		width: 750px;
	}
	
	/* 
	@type === default 
	*/
	.list-default{
		flex-direction: row;
		padding-top: 20px;
		padding-bottom: 20px;
		padding-right: 20px;
		margin-left: 20px;
		width: 730px;
		border-color: #d9d9d9;
		border-bottom-width: 1px;
		position: relative;
	}
	.list-default-img{width:50px;height: 50px;margin-right: 20px;}
	.list-default-bd{flex: 1;font-size: 32px;}
	.list-default-ft{width:50px;height: 50px;line-height:50px;font-family: iconfont;margin-left: 20px;}
	
	/* 
	@type === custom
	 */
	.list-custom{
		padding-top: 20px;
		padding-bottom: 20px;
		padding-right: 20px;
		margin-left: 20px;
		width: 730px;
		border-color: #d9d9d9;
		border-bottom-width: 1px;
		position: relative;
		flex-direction: column;
	}
	.list-custom-img{width: 160px;height:160px;}
	.list-custom-main{flex: 1;flex-direction: row;}
	.list-custom-plus{flex: 1;flex-direction: row;}
	.list-custom-content{flex: 1;margin-left: 20px;}
	/* h */
	.list-custom-h1,.list-custom-h2,.list-custom-h3{
		flex: 1;
		flex-direction: row;
		align-items: center;
	}
	/* content */
	.list-custom-h1-content,.list-custom-h2-content,.list-custom-h3-content{
		flex: 1;
		overflow: hidden;
		text-overflow:ellipsis;
	}
	/* side */
	.list-custom-h1-side,.list-custom-h2-side,.list-custom-h3-side{
		margin-left: 20px;
		overflow: hidden;
		text-overflow:ellipsis;
		white-space: nowrap;
	}
	/* h1 */
	.list-custom-h1{}
	.list-custom-h1-content{font-size: 30px;color: #000;white-space: nowrap;}
	.list-custom-h1-side{font-size: 30px;color: #000;}
	/* h2 */
	.list-custom-h2{}
	.list-custom-h2-content{font-size: 24px;color: #666;white-space: nowrap;}
	.list-custom-h2-side{font-size: 24px;color: #666;}
	/* h3 */
	.list-custom-h3{}
	.list-custom-h3-content{font-size: 20px;color:#999;line-height:30px;max-height:60px;}
	.list-custom-h3-side{font-size: 20px;color: #999;}
	/* label */
	.list-custom-label{flex-direction: row;flex: 1;margin-left: 20px;margin-top: 20px;}
	.list-custom-label-item{
		padding-top: 6px;
		padding-bottom: 6px;
		padding-left: 10px;
		padding-right: 10px;
		border-color: #ccc;
		border-width: 1px;
		border-radius: 10px;
		color: #999;
		font-size: 20px;
		margin-right: 10px;
	}
	
</style>
