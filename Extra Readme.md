## 额外修改

content改成直接上传，方便管理，而非之前的通过git submodule的方式。  
通过 snippet，将Obsidian无关的目录和文件都隐藏了，方便日常编辑。
将AboutTheGarden.md改成index.md

## 快速上传新文件

obsidian出了一个新插件quartz syncer，非常好用，可以快速上传新文件到obsidian。

## 本地调试预览

```bash
npm install
npx quartz build --serve
```

## netlify 或 vercel 部署

Build command 命令更改：
```bash
mv content/AboutTheGarden.md content/index.md && npx quartz build
```