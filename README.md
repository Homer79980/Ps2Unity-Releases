# Ps2Unity Releases

Ps2Unity 是适用于 Unity 2022.3 的纯 Editor UI 导入器。它把 PS2UI Photoshop Package 转换为可编辑的 uGUI Prefab，并处理 Sprite 复用、九宫、TextMesh Pro 字体与材质映射。

新生成的 Prefab 只使用 Unity UI、TextMesh Pro 和项目资源。PS2Unity 只在导入时工作，卸载插件后，已经生成的界面仍可继续运行。

## 前置：安装 PS2UI

先从 [PS2UI Releases](https://github.com/Homer79980/PS2UI/releases/latest) 安装 Photoshop 导出器，再在 Photoshop 中导出包含 `layout.json` 和 `sprites/` 的 Package。

## 下载 3.0.0

[打开 Ps2Unity 3.0.0 Release](https://github.com/Homer79980/Ps2Unity-Releases/releases/tag/v3.0.0)

| 文件 | 用途 |
|---|---|
| `PS2Unity-Unity-UPM-3.0.0.zip` | 推荐的 Unity Package Manager 安装包 |
| `PS2Unity-Unity-3.0.0.unitypackage` | 传统 Custom Package 安装包 |
| `PS2Unity-3.0.0-SHA256.txt` | 两个安装包的 SHA-256 |

Windows 用户也可以使用 PS2UI Engine Installer 0.1.2，一次为项目安装内置的 Ps2Unity 3.0.0。

## UPM 安装（推荐）

1. 解压 `PS2Unity-Unity-UPM-3.0.0.zip`。
2. 在 Unity 打开 `Window -> Package Manager`。
3. 点击左上角 `+ -> Add package from disk...`。
4. 选择 `com.psd2unity.uiimport/package.json`。
5. 等待依赖与插件加载完成，再从 `Tools -> PS2Unity -> 打开工作台` 进入。

## Unitypackage 安装

1. 打开 `Assets -> Import Package -> Custom Package...`。
2. 选择 `PS2Unity-Unity-3.0.0.unitypackage` 并完整导入。
3. 插件引导器会通过 Package Manager 补齐 Unity UI、TextMesh Pro 和 Newtonsoft Json。
4. 等待依赖安装和程序集重载完成，再打开工作台。

第一次在项目中使用 TextMesh Pro 时，Unity 可能打开 `TMP Importer`：点击 `Import TMP Essentials`。示例资源 `TMP Examples & Extras` 不需要导入。

## 导入操作

1. 在工作台“导入”页选择 PS2UI Package 根目录，不要选择 `sprites` 子目录。
2. 设置图片、Prefab 和公共资源目录。
3. 检查画板预览、警告、资源复用与九宫边界。
4. 点击开始导入。
5. 每个可见画板会生成对应 Prefab。会被重新导入覆盖的 Generated Prefab 不要直接挂业务脚本；业务逻辑放在外层 Prefab。

字体未绑定时仍会生成完整界面，并保留文字、字号、行高、字距、对齐和矩形。之后可在工作台“字体”页绑定项目已有的 TMP Font Asset 和材质，再刷新 Prefab。

## 从 2.1.x 升级到 3.0.0

3.0.0 不再生成 `SafeAreaFitter` 和 `PSD2UnityTextGradient`。升级前请在版本控制中搜索：

```text
SafeAreaFitter:        e9d75704fb6b4bca90eaba138d84c015
PSD2UnityTextGradient: f7c4b1a9d5e64a8f82b75d1309ce4261
```

先使用 2.1.1 重新导入旧文字渐变界面，使其改用 TMP 原生渐变。Safe Area 先替换为项目自己的实现，再关闭旧选项并重新生成。依赖 `SafeArea/Foo` 节点路径、动画或层级绑定的项目也要同步调整。

UPM 更新会替换包内容；`.unitypackage` 覆盖导入不会自动删除旧文件。确认旧 Prefab 已迁移后，再检查并删除：

```text
Assets/PSD2Unity/Runtime
Assets/UITools/Runtime
```

没有原 Photoshop Package 的旧 Prefab 无法无损自动迁移，应先保留旧组件，或由项目开发者手工替换。

## 校验下载

```powershell
Get-FileHash .\PS2Unity-Unity-UPM-3.0.0.zip -Algorithm SHA256
Get-FileHash .\PS2Unity-Unity-3.0.0.unitypackage -Algorithm SHA256
```

输出应与 `PS2Unity-3.0.0-SHA256.txt` 一致。
