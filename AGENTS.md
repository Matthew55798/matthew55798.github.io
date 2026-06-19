# 知识库导航指南

**Generated:** 2025-04-04

## OVERVIEW

中文技术知识库，基于 Docsify 构建，部署于 GitHub Pages。涵盖软件工程、投资、哲学三大领域。

## STRUCTURE

```
/
├── index.html          # Docsify 入口
├── _sidebar.md         # 导航配置
├── _coverpage.md       # 封面页
├── README.md           # 首页
├── img/                # 图片资源目录（集中管理）
│   └── 技术/          # 技术文档图片
│       ├── jvm/
│       ├── 多线程/
│       ├── 集合/
│       ├── 框架/
│       ├── 分布式/
│       ├── 微服务组件/
│       └── 数据库/rdb/
├── 软件工程/           # 技术知识（最大模块）
│   ├── 技术/          # 核心技术主题
│   ├── 常用命令/      # 运维命令速查
│   ├── 简历/          # 职业发展
│   └── 项目/          # 项目经验
├── 投资/               # 投资理论
├── 哲学/               # 哲学思考
├── 草稿本/             # 工作草稿
└── 碎纸堆/             # 杂项收藏
```

## WHERE TO LOOK

| 任务 | 位置 | 说明 |
|------|------|------|
| 查找技术文档 | `软件工程/技术/` | JVM、多线程、数据库、分布式等 |
| 查找命令速查 | `软件工程/常用命令/` | Linux、Docker、Oracle |
| 添加新章节 | 编辑 `_sidebar.md` | 按层级缩进2空格 |
| 修改首页 | `_coverpage.md` | 封面内容 |
| 部署站点 | `.github/workflows/deploy.yml` | 推送到main自动部署 |

## CONVENTIONS

### 文件命名
- 中文标题，空格分隔（如：`Docker 安装 MySQL 指南.md`）
- 章节编号用 `第X章` 前缀

### 图片资源
- 图片资源统一放在 `img/` 下，按模块/主题分子目录；不要把图片散放在 Markdown 文件旁边。
- 图片目录和文件名使用小写短横线命名（kebab-case），不要使用空格、下划线或大小写混杂；中文主题目录可以保留中文。
- 图片文件名必须有语义，避免 `image.png`、`image 1.png` 这类泛名；示例：`thread-states.png`、`lock-upgrade.png`。
- Markdown 中引用图片必须使用从站点根开始的绝对路径：`![说明](/img/技术/多线程/thread-states.png)`。
- 不要使用相对路径（如 `./image.png`、`../img/foo.png`），Docsify 的 hash 路由会导致解析位置不稳定。
- 如果历史文件名确实包含空格，必须先改名为短横线文件名，不要在 Markdown 中用 `%20` 继续引用。

### 导航配置 (_sidebar.md)
- 缩进使用**2空格**（非Tab）
- 层级通过缩进深度控制
- 链接格式：`[显示文字](/路径/文件名.md)`

### 内容组织
- README.md 作为目录索引
- 技术文档放 `软件工程/技术/`
- 草稿/未完成内容放 `草稿本/`
- 收藏/杂项放 `碎纸堆/`
- 图片资源放 `img/`，按模块/主题分子目录

## COMMANDS

```bash
# 本地预览（需安装 docsify-cli）
docsify serve .

# 部署（自动）
git push origin main
```

## ANTI-PATTERNS

- **不要**直接在根目录添加 Markdown 文件（非索引类）
- **不要**使用 Tab 缩进 sidebar（会导致渲染失败）
- **不要**修改 `index.html` 中的 CDN 路径除非必要
- **不要**使用 `image.png`、`image 1.png`、带空格或下划线的图片文件名
- **不要**在 Markdown 中使用图片相对路径
- **不要**提交 `node_modules/` 或构建产物

## NOTES

- 搜索支持中文，深度6级
- 代码块支持 Java/SQL/Bash 高亮
- 分页插件已启用（上一篇/下一篇）
- 站点统计使用 busuanzi
