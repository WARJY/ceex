# listc

 > Ceex listc组件
 
## 规则
- 包含常用list及高扩展性的list组件
- 配置对象结构中的每个字段对应相应的部分，若不传入则不会出现

## 使用方法
```vue
<template>
	<listc title="我是标题" :items="listItems" :defaultStyle="defaultStyle"></Listc>
	<listc type="custom" title="我是custom" :items="listItems2" :customStyle="customStyle" @tap=""></Listc>
</template>
<script>
	export default {
		data(){
			return {
				//default对象结构
				listItems:[
					{src:"https://vuejs.org/images/logo.png",title:"设置",url:" "}
				],
				//default自定义样式结构
				defaultStyle:{
					img:{},
					title:{},
					arrow:{}
				},
				//custom对象结构
				listItems2:[
					{
						src:"https://vuejs.org/images/logo.png",
						h1:"设置",
						h2:"我是设置",
						h3:"我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内容我是内",
						h1side:"h1side",
						h2side:"h2side",
						h3side:"h3side",
						label:["label1","label2"],
						url:" "
					}
				]
				//custom自定义样式结构
				customStyle:{
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
	}
</script>
```

## 可配置参数
| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| items | `Array` |`Y`| `[]` | list配置属性 |
| type | `String` |`N`| `default` | list类型 |
| title | `String` |`N`| `` | 列表大标题，若传入此属性则显示 |
| defaultStyle | `Object` |`N`| `{}` | 自定义default样式 |
| customStyle | `Object` |`N`| `{}` | 自定义custom样式 |

## Event
```
@tap	banner点击事件		若传入url则直接跳转然后触发此事件，否则直接触发事件
```
e = {
	{src:"https://vuejs.org/images/logo.png",title:"设置",url:" "}
}
```