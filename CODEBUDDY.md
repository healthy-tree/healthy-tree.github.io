# CODEBUDDY.md

## 项目概述

个人技术博客站点，基于 **Hexo 8.x** 静态站点生成器 + **NexT 8.x** 主题，内容以 Java / Spring 相关技术笔记为主，语言为 `zh-CN`。

- 线上地址：https://healthy-tree.github.io/
- 仓库：`git@github.com:healthy-tree/healthy-tree.github.io.git`
- 默认分支：`main`（`develop` 为开发分支，通过 PR 合入）

## 技术栈

| 项 | 值 |
| --- | --- |
| 生成器 | `hexo@^8.1.2`（要求 **Node >= 20.19.0**） |
| 主题 | `hexo-theme-next@^8.29.0`（npm 安装，位于 `node_modules`，`themes/` 目录为空是正常的） |
| 渲染器 | marked（Markdown）、asciidoc、ejs、pug、stylus |
| 生成器插件 | archive / category / tag / index / feed / searchdb / sitemap |
| 其他插件 | `hexo-word-counter`（字数与阅读时长） |
| 评论系统 | 主题内置 **utterances**（评论存 GitHub Issues，需仓库装 utterances App） |
| 自定义样式 | `source/_data/styles.styl`（段首缩进 + `text-autospace` 中英文间距） |
| CI | GitHub Actions（`.github/workflows/pages.yml`），Node 22 LTS |

## 常用命令

```bash
npm run server   # 本地预览 hexo server
npm run build    # hexo generate，产出到 public/
npm run clean    # hexo clean
npx hexo new "标题"        # 新建文章 → source/_posts/标题.md
npx hexo new draft "标题"  # 新建草稿 → source/_drafts/
npx hexo new page "about"  # 新建页面
```

## 目录结构

```
_config.yml        站点主配置（注意：Hexo 原生配置）
_config.next.yml   NexT 主题配置（NexT 8 支持站点根目录下的独立配置文件）
scaffolds/         文章模板（post / draft / page）
source/_posts/     已发布文章（当前 6 篇）
source/_drafts/    草稿
themes/.gitkeep    主题由 npm 提供，此目录保持为空
public/            构建产物（已 gitignore）
db.json            本地索引缓存（已 gitignore）
```

## 约定

- **Front matter**（见 `scaffolds/post.md`）：`title`、`date`、`tags`；分类使用默认的 `uncategorized`。
- **文件名**：`new_post_name: :title.md`，中文/英文标题直接作为文件名，不做大小写或空格转换（`filename_case: 0`）。
- **永久链接**：`:year/:month/:day/:title/`。
- 文章内容为中文，代码注释可视情况保留英文；技术术语（类名、注解、配置项）保持原文，不要翻译。
- 修改主题外观优先改 `_config.next.yml`，不要直接改 `node_modules/hexo-theme-next`。注意 `scheme` 只在 `_config.next.yml` 中生效（当前为 `Muse`），`_config.yml` 根部的 `scheme: Pisces` 是无效配置。
- 修改依赖后需同步 `package-lock.json`，CI 使用 `npm ci`。

## 发布流程

推送到 `main` 会触发 `.github/workflows/pages.yml`：安装依赖 → `npm run build` → 上传 `./public` → 部署到 GitHub Pages。

本地提交前建议先跑一次 `npm run clean && npm run build` 确认能正常生成。

## 注意事项

- `.gitignore` 已忽略 `public/`、`node_modules/`、`db.json`、`.idea/`，不要把这些提交进仓库。
- `db.json` 已存在于工作区但被忽略，本地搜索索引依赖它，缺失时 `hexo clean` 后会重新生成。
- 仓库根目录存在 `.DS_Store` 与 `.idea/`，清理时不要误删源文件。
