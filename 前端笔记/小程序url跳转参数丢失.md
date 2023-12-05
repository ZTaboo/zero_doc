> 使用 `encodeURIComponent` 进行编码然后用 `decodeURIComponent` 解码


```javascript
// 发送
toShopInfo(e) {
			let urlData = JSON.stringify(e.currentTarget.dataset.info);
			wx.navigateTo({
				url: "/pages/shopInfo/shopInfo?info=" + encodeURIComponent(urlData),
			});
		},
```

```javascript
// 接收
onLoad(options) {
		console.log("options:", decodeURIComponent(options.info));
	},
```
