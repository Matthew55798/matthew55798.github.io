# 知识库导航指南

**Generated:** 2025-04-04
**Updated:** 2026-07-05

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
│       ├── ai-agent/
│       ├── jvm/
│       ├── redis/
│       ├── 多线程/
│       ├── 集合/
│       ├── 框架/
│       ├── 分布式/
│       ├── 微服务组件/
│       ├── 机器学习/
│       └── 数据库/rdb/
├── 软件工程/           # 技术知识（最大模块）
│   ├── 技术/          # 核心技术主题
│   │   ├── Java/      # JVM、多线程、集合等 Java 基础
│   │   ├── 常用框架/  # Spring、Spring Boot、MyBatis 等
│   │   ├── 数据库/    # Redis、Elasticsearch、RDB 等
│   │   ├── 系统设计基础/
│   │   ├── 分布式系统/
│   │   ├── 微服务架构/
│   │   ├── 高可用/
│   │   ├── 高性能/
│   │   ├── 可观测/
│   │   ├── AI/        # AI工程、LLM业务实战、Vibe Coding、AI Agent、机器学习
│   │   │   ├── AI工程/
│   │   │   ├── LLM业务实战/
│   │   │   ├── Vibe Coding/
│   │   │   ├── 菜鸟教程-AI Agent/
│   │   │   └── 菜鸟教程-机器学习/
│   │   └── 参考资料/  # 本地资料归档，不展示在 sidebar
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
| 查找本地参考资料 | `软件工程/技术/参考资料/` | PDF、离线文档、外部资料快照 |
| 查找阿里巴巴开发规范 | `软件工程/技术/参考资料/阿里巴巴 Java 开发手册 黄山版.pdf` | 官方 alibaba/p3c 仓库下载的黄山版手册，许可见同目录 `alibaba-p3c-license.txt` |
| 查找 yudao / ruoyi-vue-pro 文档 | `软件工程/技术/参考资料/20260429/` | 优先检索本地 `yudao-cloud/`、`ruoyi-vue-pro/`，因为网站部分文档需要登录，在线检索不稳定 |
| 添加新章节 | 编辑 `_sidebar.md` | 按层级缩进2空格 |
| 修改首页 | `_coverpage.md` | 封面内容 |
| 部署站点 | `.github/workflows/deploy.yml` | 推送到main自动部署 |

## CONVENTIONS

### 文件命名
- Markdown 文档文件名尽量不要使用空格，使用下划线 `_` 分隔语义段，降低 `_sidebar.md` 和文档互链维护成本。
- 新增文档推荐格式：`Docker_安装_MySQL_指南.md`、`第1章_Spring.md`、`第1章_大语言模型简介.md`。
- 章节编号用 `第X章_` 前缀；历史文件如果已有空格，迁移时必须同步更新 `_sidebar.md` 和所有 Markdown 引用。

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
- 新增 sidebar 链接优先指向无空格文件名，避免在 URL 中维护 `%20`。
- README 文件不纳入 `_sidebar.md`，也不要以“目录索引”等名称作为 sidebar 条目展示。
- `草稿本/` 和 `碎纸堆/` 不属于成体系知识区，不展示在 `_sidebar.md` 中；内容保留为可搜索或通过直链访问。
- `软件工程/技术/参考资料/` 是资料归档目录，也不展示在 `_sidebar.md` 中；正文可以按需直链引用。

### 内容组织
- README.md 可作为首页或分区说明文件
- 技术文档放 `软件工程/技术/`
- 本地参考资料放 `软件工程/技术/参考资料/`
- yudao / ruoyi-vue-pro 相关文档优先从 `软件工程/技术/参考资料/20260429/` 检索，不要默认依赖在线文档。
- 阿里巴巴《Java 开发手册（黄山版）》位于 `软件工程/技术/参考资料/阿里巴巴 Java 开发手册 黄山版.pdf`；引用时标注来源为官方 `alibaba/p3c`，避免整本大段转载。
- 草稿/未完成内容放 `草稿本/`
- 收藏/杂项放 `碎纸堆/`
- 图片资源放 `img/`，按模块/主题分子目录
- 目录结构变化不会自动同步到最外层 `AGENTS.md` 的 `STRUCTURE`；移动、增加、删除一级或关键二级目录后，应手动更新这里。
- 本项目统一使用大写 `AGENTS.md` 作为 agent 导航文件名；不要新增小写 `AGENTS.md`。

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
- **不要**给新增 Markdown 文档文件名使用空格；使用下划线 `_` 分隔
- **不要**使用 `image.png`、`image 1.png`、带空格或下划线的图片文件名
- **不要**在 Markdown 中使用图片相对路径
- **不要**提交 `node_modules/` 或构建产物

## NOTES

- 搜索支持中文，深度6级
- 代码块支持 Java/SQL/Bash 高亮
- 分页插件已启用（上一篇/下一篇）
- 站点统计使用 busuanzi
