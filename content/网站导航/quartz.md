---
{"publish":true,"permalink":"/网站导航/quartz.md","aliases":"quartz","created":"2025-07-30T16:05:02.793+08:00","modified":"2025-07-30T18:24:10.527+08:00","tags":["obsidian"],"cssclasses":""}
---

自动发布
 - 直接 fork [quartz仓库](https://github.com/jackyzha0/quartz) 
 - 拉取自己的仓库
 - 在 content 目录下新增笔记
 - 在 obsidian 中安装 [quartz-syncer](https://github.com/saberzero1/quartz-syncer) 插件，按照文档进行相关配置
 - 新建GitHub Action 工作流， 填入配置即可 (修改自官方)

```yml
name: Deploy Quartz site to GitHub Pages

on:

  push:

    branches:

      - v4

  workflow_dispatch: # 允许手动运行

permissions:

  contents: read

  pages: write

  id-token: write

concurrency:

  group: "pages"

  cancel-in-progress: false

jobs:

  build:

    runs-on: ubuntu-22.04

    steps:

      - uses: actions/checkout@v4

        with:

          fetch-depth: 0 # Fetch all history for git info

      - uses: actions/setup-node@v4

        with:

          node-version: 22

      - name: Install Dependencies

        run: npm ci

      - name: Build Quartz

        run: npx quartz build

      - name: Upload artifact

        uses: actions/upload-pages-artifact@v3

        with:

          path: public

  deploy:

    needs: build

    environment:

      name: github-pages

      url: ${{ steps.deployment.outputs.page_url }}

    runs-on: ubuntu-latest

    steps:

      - name: Deploy to GitHub Pages

        id: deployment

        uses: actions/deploy-pages@v4
```