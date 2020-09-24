# VUE

## 环境变量配置

>vue 环境变量配置
>
>npm config list
>
>prefx 路径 配置到path

## props传参

```js
//设置默认值
//数组
sugestion: {
  type: Array,
  default: () => []
}
//对象
sugestion: {
  type: Object,
  default: () => 
}
//字符串
sugestion: {
  type: String,
  default: 'string'
}
```

