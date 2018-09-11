# banner

 > Ceex banner组件
 
## 规则
- 常用的banner组件

## 使用方法
```vue
<template>
   <banner :setting="bannerSetting" @switch="" @tap="" />
</template>
<script>
	export default {
		data(){
			return {
				bannerSetting:[
					{src: "https://vuejs.org/images/logo.png",url: ""},
					{src: "https://vuejs.org/images/logo.png",url: ""}
				]
			}
		}
	}
</script>
```

## 可配置参数
| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| Items | `Array` |`Y`| `[]` | banner配置属性 |
| width | `Number` |`N`| `750` | banner宽度 |
| height | `Number` |`N`| `270` | banner高度 |

## Event
@switch	banner切换事件
```
e = {
	src:"https://vuejs.org/images/logo.png",url: ""
}
```
@tap	banner点击事件		若传入url则直接跳转然后触发此事件，否则直接触发事件
```
e = {
	src:"https://vuejs.org/images/logo.png",url: ""
}
```