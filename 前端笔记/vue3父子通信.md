```javascript
// 获取父级方法或函数调用
const emit = defineEmits(['cancel', 'resetData']);
// 获取父级绑定的变量
const props = defineProps({
	visible: {
		type: Boolean,
		default: false
	},
	title: {
		type: String,
		default: ''
	}
});
// 暴露方法给父级用于ref调用子组件方法 
defineExpose({
	getEditData,  
})
```
