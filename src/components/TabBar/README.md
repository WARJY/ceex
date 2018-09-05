# tabbar

 > Ceex 标签栏组件
 
## 规则
- 底部导航栏组件
- page组件请嵌套在tabbar中使用
- 页面slot需对应setting中的name

## 使用方法
```vue
<template>
  <tab-bar :iconfont="" :setting="navSetting" :index="0" :styleDefault="styleDefault" :styleActive="" @switch="">
   	<page slot="index" :isShow="showindex"></page>
   	<page slot="my" :isShow="showmy"></page>
  </tab-bar>
</template>
<script>
export default {
	data:{
		iconfont:"https://xxxx.ttf"
		//字体图标时的setting结构
		iconfontSetting = {
			{name: "index",icon: "\ue751;",title: "首页"},
			{name: "my",icon: "\ue75e;",title: "我的"}
		},
		//图片图标时的setting结构
		imgSetting = {
			{name："index",icon: "https://vuejs.org/images/logo.png",activeIcon: " ",title: "首页"},
			{name："my",icon: "https://vuejs.org/images/logo.png",activeIcon: " ",title: "首页"}
		},
		//传入的style结构
		styleDefault: {
			icon: {
				color: '#333'
			},
			title: {
				color: '#333'
			}
		}
	}
}
</script>
```

## 可配置参数
| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| setting | `Array` |`Y`| `[]` | tabbar内容区域设置 |
| index | `Number` |`N`| `0` | 控制当前内容区域的index |
| duration | `Number` |`N`| `0` | 内容区域切换动画时间 |
| iconfont | `String` |`N`| `` | 开启此属性则使用iconfont结构的tabbar，若使用图片请不要传入此属性 |
| styleDefault | `Object` |`N`| `{}` | 默认的tabbar样式 |
| styleActive| `Object` |`N`| `{}` | 活动的tabbar样式 |

## 插槽
| name | Description |
|------|-------------|
| [name] | 传入setting中name字段对应的值才可以将内容区域关联到相应的tab |
| [name + 'Bar'] | 传入setting中name字段 + 'Bar'可以对tab标签栏的内容进行修改，如indexBar对应index |

## Event
@switch	tabbar点击切换事件	初始化时会根据初始index值触发一次
```
e = {
	//load仅在页面首次加载时出现，值为setting对象中对应name的值
	load: {name:"index",icon: "\ue751;",title: "首页"},
	//每次切换都会传入，值为setting对象中对应name的值
	target: {name:"index",icon: "\ue751;",title: "首页"},
}
```