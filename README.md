# HelloWorld

[![Pre Release](https://github.com/NKU100/helloworld/actions/workflows/pre-release.yml/badge.svg)](https://github.com/NKU100/helloworld/actions/workflows/pre-release.yml)
[![Release](https://github.com/NKU100/helloworld/actions/workflows/release.yml/badge.svg)](https://github.com/NKU100/helloworld/actions/workflows/release.yml)

一个基于 CMake + vcpkg 的 C++ 项目，使用 MSVC 静态链接，支持 x86/x64 双架构构建，并通过 GitHub Actions 实现自动化 CI 预发布和正式发版。

## 功能

- 使用 [fmt](https://github.com/fmtlib/fmt) 库格式化输出
- 基于 Git Tag 自动管理版本号
- 输出版本信息和 Git Commit Hash

## 环境要求

- Windows
- [Visual Studio 2022](https://visualstudio.microsoft.com/)（需包含 C++ 桌面开发工作负载）
- [CMake](https://cmake.org/) ≥ 3.31
- [Ninja](https://ninja-build.org/)
- [vcpkg](https://github.com/microsoft/vcpkg)（需设置 `VCPKG_ROOT` 环境变量）

## 构建

### 克隆仓库

```bash
git clone --recursive <repo-url>
cd helloworld
```

### 安装依赖

```bash
vcpkg install --triplet x86-windows-static
vcpkg install --triplet x64-windows-static
```

### 编译

项目提供了 CMake Presets，可直接使用：

```bash
# x86 Release
cmake --preset x86-release
cmake --build --preset x86-release

# x64 Release
cmake --preset x64-release
cmake --build --preset x64-release
```

可用的预设：

| 预设 | 架构 | 配置 |
|------|------|------|
| `x86-debug` | x86 | Debug |
| `x86-release` | x86 | Release |
| `x64-debug` | x64 | Debug |
| `x64-release` | x64 | Release |

> **注意**：需要先通过 VsDevCmd 设置好对应架构的编译环境，例如：
> ```cmd
> call "C:\Program Files\Microsoft Visual Studio\2022\Enterprise\Common7\Tools\VsDevCmd.bat" -arch=x86 -host_arch=amd64
> ```

## 项目结构

```
helloworld/
├── CMakeLists.txt              # 顶层 CMake 配置
├── CMakePresets.json           # CMake 预设（x86/x64 Debug/Release）
├── vcpkg.json                  # vcpkg 依赖声明
├── vcpkg-configuration.json    # vcpkg 注册表配置
├── COMMIT_CONVENTION.md        # 提交规范
├── .github/
│   └── workflows/
│       ├── build.yml           # 可复用的构建工作流
│       ├── pre-release.yml     # CI 预发布工作流
│       └── release.yml         # 正式发版工作流
└── helloworld/                 # 子模块 - 应用源码
    ├── CMakeLists.txt          # 子模块 CMake 配置（含版本管理）
    ├── helloworld.cpp          # 主程序源码
    └── version.h.in            # 版本头文件模板
```

## CI/CD

项目使用 GitHub Actions 实现自动化构建与发布，支持 CI 预发布和正式发版两种模式。

### 工作流架构

```
pre-release.yml ─┐
                 ├──► build.yml (可复用)
release.yml ────┘     ├── vcpkg-install (x86 + x64 并行)
                      └── build (x86 + x64 并行)
```

### Pre Release（CI 预发布）

| 触发条件 | 产物 | Release |
|---|---|---|
| 推送到 `main` 分支 | `HelloWorld-x86.exe`<br>`HelloWorld-x64.exe` | tag: `ci`<br>prerelease: true |
| 手动触发 | | |

- Changelog：上一个 `v*` tag → 当前 HEAD（`--unreleased`）
- 每次构建会更新 `ci` tag 到最新提交

### Release（正式发版）

| 触发条件 | 产物 | Release |
|---|---|---|
| 推送 `v*` tag（如 `v1.0.0`） | `HelloWorld-v1.0.0-x86.exe`<br>`HelloWorld-v1.0.0-x64.exe` | tag: `v1.0.0`<br>prerelease: false |
| 手动触发 | | |

- Changelog：上一个 `v*` tag → 当前 `v*` tag（`--latest`）
- 产物文件名带版本号

### 发版流程

```bash
# 正式发版
git tag v1.0.0
git push origin v1.0.0
```

## 提交规范

本项目遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范，详见 [COMMIT_CONVENTION.md](COMMIT_CONVENTION.md)。

## License

MIT
