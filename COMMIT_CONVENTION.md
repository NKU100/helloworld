# 提交规范

本项目遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范。

## 提交格式

```
<type>: <description>

[body]
```

### Type 类型

| 类型 | 说明 |
|------|------|
| `feat` | 新功能 |
| `fix` | 修复 Bug |
| `refactor` | 代码重构（不涉及功能变更或 Bug 修复） |
| `chore` | 构建、CI、依赖等维护性改动 |
| `docs` | 文档变更 |
| `perf` | 性能优化 |
| `test` | 测试相关 |

## 子模块更新规范

更新子模块引用时，**必须**在 commit body 中列出子模块的具体变更内容，以便 changelog 能够反映子模块的改动。

### 格式

```
chore: 更新 <子模块名> 子模块引用

子模块更新内容:
- <type>: <子模块 commit message 1>
- <type>: <子模块 commit message 2>
```

### 示例

```
chore: 更新 helloworld 子模块引用

子模块更新内容:
- refactor: 重构版本管理变量名并新增 GIT_COMMIT_HASH 支持
- feat: 新增输出 git commit hash 信息
```

### 快速获取子模块变更摘要

在 `git add <submodule>` 之后、提交之前，运行以下命令查看子模块的新增 commits：

```bash
git diff --cached --submodule=log
```

输出示例：

```
Submodule helloworld ff0883a..12d89a8:
  > feat: 新增输出 git commit hash 信息
  > refactor: 重构版本管理变量名并新增 GIT_COMMIT_HASH 支持
```

将输出内容整理后写入 commit body 即可。
