## 设置淘宝镜像

> 打包过程中会下载插件安装，如果没有设置镜像，直接从gihub下载，几乎会卡死。


- 执行命令`npm config edit`
- 将以下代码复制进文件中

```bash
electron_mirror=https://npm.taobao.org/mirrors/electron/
electron-builder-binaries_mirror=https://npm.taobao.org/mirrors/electron-builder-binaries/
```

## 常见问题

### 找不到electron.js

```bash
"build":{
   "extends": null,
 }
```

### react + electron打包找不到js等相关静态资源

`package.json`增加`homepage` 参数

入口文件：main.js内资源路径在生产环境下使用如下参数

```javascript
const urlLocation = isDev ? 'http://localhost:3000' : url.format({
        pathname: path.join(__dirname, './build/index.html'),
        protocol: 'file:',
        slashes: true
    })
```
