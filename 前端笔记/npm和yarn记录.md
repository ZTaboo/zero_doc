> pnpm配置和npm配置方式一致


- npm查看当前镜像源

```bash
npm get registry
```

- yarn查看当前镜像源

```bash
yarn config get registry
```

- 安装cnpm

```bash
npm install -g cnpm --registry=https://registry.npmmirror.com
```

- yarn官方源

```latex
https://registry.yarnpkg.com
```

- 淘宝镜像官网

[node镜像](https://developer.aliyun.com/mirror/NPM?spm=a2c6h.13651102.0.0.4c771b11cMgyMv)

- 最新镜像地址

```bash
<http://npm.taobao.org> => <https://npmmirror.com>
<http://registry.npm.taobao.org> => <https://registry.npmmirror.com>
# 前者为旧的镜像地址后者为新地址
```

- 设置镜像源

```bash
# npm
npm config set registry    <mirror_addresses>

#yarn
yarn config set registry https://registry.npmmirror.com
```

## volta使用记录

> 目前无法删除node工具 2022/06/01


删除方法：

```
# Linux and MacOS删除
~/.volta/tools/image/node/
# windows删除
%LOCALAPPDATA%\\\\Volta\\\\tools\\\\image\\\\node\\\\
```

### 设置electron镜像

```bash
yarn config set electron_mirror "https://registry.npmmirror.com/electron"
```
