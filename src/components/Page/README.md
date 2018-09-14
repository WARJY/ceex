# page

 > Ceex 页面组件
 
## 规则
- 该组件依赖于weex-ui组件库
- 页面根元素，所有内容都嵌套在垂直scroller下,自带首次加载，下拉刷新及上拉加载功能
- 更换内置加载组件需将对应slot属性置于true并传入具名slot
- 页面中不能再嵌套垂直scroller

## 使用方法
```vue
<template>
   <page :isShow="true">
   	<div slot="nav"></div>
   </page>
</template>
```

## 可配置参数
| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| isShow | `Bool` |`Y`| `false` | 是否显示,不显示时默认显示加载|
| refreshing | `Bool` |`N`| `false` | 控制下拉刷新状态的显示 |
| loading | `Bool` |`N`| `false` | 控制下拉加载状态的显示 |
| topBarHeight | `Number` |`N`| `90` | 导航栏高度，Page会在页面顶部减去对应的高度 |
| navBarHeight | `Number` |`N`| `98` | 标签栏高度，Page会在页面底部减去对应的高度 |
| toTop | `Bool` |`N`| `false` | 是否开启回到顶部按钮 |
| refreshSlot | `Bool` |`N`| `false` | 是否开启自定义下拉刷新slot |
| loadSlot | `Bool` |`N`| `false` | 是否开启自定义上拉加载slot |
| pageLoadSlot | `Bool` |`N`| `false` | 是否开启自定义页面加载slot |

## 插槽
| name | Description |
|------|-------------|
| refreshSlot | 使用前需开启refreshSlot属性，下拉刷新时显示在页面顶部 |
| loadSlot | 使用前需开启loadSlot属性，下拉加载时显示在页面底部 |
| pageLoadSlot | 使用前需开启pageLoadSlot属性，isShow为false时自动显示 |

## Event
@scroll	页面滚动事件
```
e = {
	//列表内容尺寸
	contentSize:{
		width : Number,
		height : Number
	},
	//列表偏移量
	contentOffset:{
		x:Number,
		y:Number
	}
}
```
@pullDown	页面下拉事件
```
e = {
	dy:Number,	//前后两次回调滑动距离的差值
	pullingDistance:Number,	//下拉的距离
	viewHeight:Number,	//组件高度
	type:"pullingdown"
}
```
@refreshing	下拉刷新触发

@loadining	上拉加载触发