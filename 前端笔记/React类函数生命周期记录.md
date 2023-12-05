## 生命周期

图形演示：

- 
`componentDidMount`

   - 初始化仅执行一次
- 
`componentDidUpdate`

   - 在更新后会被立即调用。首次渲染不会执行此方法。
   - 当组件更新后，可以在此处对 DOM 进行操作。如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求。（例如，当 props 未发生变化时，则不会执行网络请求）。
- 
`componentWillUnmount`

   - 会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 componentDidMount() 中创建的订阅等。
