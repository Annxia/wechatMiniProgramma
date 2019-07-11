## 什么是uni-app?  
传统的h5只有一端，即浏览器。而uni-app可跨7端，虽仍属前端，与传统的h5有不同。
	
#### 网络模型的变化  
b/s，服务端代码混合在页面里  --->  c/s,前后端分离，通过ajax获取json数据  

#### 文件类型的变化  
.html  --->  .vue
	
#### 文件内代码架构的变化
一个html大节点，里面有script和style节点  --->  
template是一级节点，用于写tag组件，script和style是并列的一级节点，也就是有3个一级节点。 

以前:
```JavaScript
	<!DOCTYPE html>  
	<html>  
		<head>  
			<meta charset="utf-8" />  
			<title></title>  
			<script type="text/javascript">  
	
			</script>  
			<style type="text/css">  
	
			</style>  
		</head>  
		<body>  
	
		</body>  
	</html>  

```

现在: **vue单文件组件规范sfc**  
```JavaScript
	<template>  
		<view>  
		注意必须有一个view，且只能有一个根view。所有内容写在这个view下面。  
		</view>  
	</template>  
	
	<script>  
		export default {  
	
		}  
	</script>  
	
	<style>  
	
	</style>  
```  

#### 外部文件引用方式变化  
script src、link href引入外部的js和css  --->  
es6的写法，import引入外部的js模块(注意不是文件)或css
	
	