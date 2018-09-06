# menu

 > Ceex 主页菜单组件
 
## 规则
- 一般用于app首页的菜单按钮组

## 使用方法
```vue
<template>
   <menu row="5" setting="menuSetting" styleDefault="styleDefault" @tap="" />
</template>
<script>
	export default {
		data(){
			return {
				menuSetting:[
					{name:"lunch",icon:"xxx.png",title:"午餐",url:"xxx"},
					{name:"dinner",icon:"xxx.png",title:"晚餐",url:"xxx"},
				],
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
	}
</script>
```

## 可配置参数
| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| setting | `Array` |`Y`| `[]` | menu设置属性 |
| row | `Number` |`Y`| `5` | 每行显示几个按钮 |

## Event
@tap	按钮点击事件		若传入url则直接跳转，否则触发tap事件
```
e = {
	name:"lunch",icon:"xxx.png",url:"xxx"
}
```