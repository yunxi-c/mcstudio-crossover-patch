# MCStudio CrossOver 兼容补丁

为 macOS 上的 CrossOver 容器提供 MCStudio / ModPC OpenGL 兼容补丁。双击脚本后只需两步：**选择容器 → 注入或移除**。

## 下载

建议下载 [MCStudio-CrossOver-Patch.zip](MCStudio-CrossOver-Patch.zip)，解压后双击 `MCStudio容器补丁.command`。压缩包保留脚本的可执行权限。

## 环境要求

- macOS、Python 3、已创建的 CrossOver 容器。
- 容器内需安装 .NET 和 Visual C++ 运行环境，供 MCStudio 使用。
- **允许所有 CrossOver 版本尝试注入**；仍需具备 x86_64 Wine 运行库结构。当前驱动基于 **26.3** 构建，仅该版本完成过运行验证，其他版本兼容性待验证。
- 在 Apple 芯片 Mac 上使用 x86_64 Wine 时需要 Rosetta。
- CrossOver、官方我的世界开发者启动器、游戏和编辑器需自行获取，本项目不提供这些软件。

## 使用方法

1. 保存工作，关闭目标容器中的游戏、编辑器和启动器。
2. 双击 `MCStudio容器补丁.command`。
3. 选择容器编号。
4. 选择 **1 注入** 或 **2 移除**。
5. 注入后继续使用官方启动器图标打开启动器、启动 ModPC。

可以先对空容器注入，再安装所需组件和官方启动器。启动器安装完成后，再对同一容器执行一次注入，以连接新生成的菜单入口；编辑器不必预先安装。

## 修改内容

补丁在选定容器的 `modpc-opengl` 目录内准备私有运行库与备份，并修改：

- 私有运行库的 `winemac.so`：OpenGL 兼容与实验性内嵌画面处理。
- `cxbottle.conf`：运行库路径、OpenGL 和中文编码设置。
- 已安装的官方启动器快捷方式及 `cxmenu.conf` 中对应的菜单入口。

菜单使用正常 Wine 加载路径，打开实际启动器后辅助入口自动退出，以避免持续显示两个运行图标。

本项目不修改 `CrossOver.app`、游戏 EXE 或 Lightning 插件，不包含账号、地图、登录记录或 CrossOver 完整运行库。

## 验证范围

当前版本是**实验版本**，代码不设 CrossOver 版本限制。已在 CrossOver 26.3 的测试容器中确认：从官方图标启动 ModPC，关闭启动器后重新打开并再次启动成功；辅助入口能自动退出，实际启动器继续运行。双图标修正后的 Dock 外观仍待用户确认。

已在隔离的配置测试中检查空容器先注入、后装启动器再注入、重复注入、移除恢复和保留无关菜单配置；这些检查不等同于全部实际容器的运行测试。编辑器真正内嵌的画面、输入与长期稳定性仍在实验中，不能由 ModPC 启动成功推定全部可用。

## 移除与备份

再次运行脚本，选择相同容器，再选择 **2 移除**。脚本恢复注入前的相关配置、驱动和菜单入口；私有运行库和备份文件保留。无关配置变更会尽量保留；如果补丁管理的文件或入口被其他程序修改，脚本会停止，避免覆盖。

## 源码与许可证

脚本是自包含的，源码与驱动载荷均内置。可导出到一个尚不存在的目录：

```sh
./MCStudio容器补丁.command --extract-source ./mcstudio-patch-source
```

也可下载 [源码包](mcstudio-crossover-source.zip)。其中包含 `manage.py`、`source/patch-driver.py`、`source/cocoa_shared_gl.m`、GPU 检查源码与 `source/BUILD.md`。

驱动基于 CodeWeavers 的 Wine 26.3 源码修改。上游源码地址和构建说明见 [SOURCE.md](SOURCE.md)。发布采用 LGPL-2.1-or-later，保留 Wine 原有版权与许可证。项目与网易、CodeWeavers 无官方关联。
