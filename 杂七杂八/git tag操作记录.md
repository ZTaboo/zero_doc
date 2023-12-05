## 创建tag

```bash
git tag -a v1.0.0 -m "初步完成"
```

## 提交本地tag到远程

```bash
git push origin --tags
```

## 拉取远程tag到本地

```bash
git fetch --tags
```

## 删除本地tag

```bash
git tag -d <tagName>
```

## 删除远程tag

```bash
git push origin :refs/tags/<tagName>
```
