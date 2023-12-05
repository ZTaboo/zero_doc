修改 `tailwind.config.js` 文件. **禁止tailwindcss的默认属性,添加corePlugins对象，并设置preflight为false**:

```jsx
corePlugins: {
        preflight: false
    },
```
