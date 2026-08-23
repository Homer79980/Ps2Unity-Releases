# Ps2Unity Releases

Ps2Unity 是 Unity 2022.3+ 编辑器插件，用于把 PS2UI Photoshop Package 导入为可编辑的 uGUI Prefab，并处理图片复用、九宫、TextMesh Pro 字体与材质映射。

## 前置：安装 PS2UI

本仓库不包含 Photoshop 插件。先从 [PS2UI Releases](https://github.com/Homer79980/PS2UI/releases/latest) 安装 `PS2UI-Photoshop-<版本>.ccx`，在 Photoshop 中导出包含 `layout.json` 和 `sprites/` 的 Package。

## 下载

[打开最新 Release](https://github.com/Homer79980/Ps2Unity-Releases/releases/latest)

| 文件 | 用途 |
|---|---|
| `PS2Unity-Unity-UPM-2.1.1.zip` | 推荐的 Unity Package Manager 安装包 |
| `PS2Unity-Unity-2.1.1.unitypackage` | 传统 Custom Package 安装包 |
| `PS2Unity-2.1.1-SHA256.txt` | 两个安装包的 SHA-256 |

## UPM 安装（推荐）

1. 解压 `PS2Unity-Unity-UPM-2.1.1.zip`。
2. 在 Unity 打开 `Window -> Package Manager`。
3. 点击左上角 `+ -> Add package from disk...`。
4. 选择 `com.psd2unity.uiimport/package.json`。
5. 等待编译完成，从 `Tools -> PS2Unity -> 打开工作台` 进入。

## Unitypackage 安装

1. 打开 `Assets -> Import Package -> Custom Package...`。
2. 选择 `PS2Unity-Unity-2.1.1.unitypackage` 并完整导入。
3. 插件引导器会通过 UPM 补齐 Unity UI、TextMesh Pro 和 Newtonsoft Json。
4. 等待依赖安装和程序集重载完成，再打开工作台。

旧工程若保留 `Assets/UITools` 等手工复制版本，应先移除旧副本，避免同一程序集重复。升级后的第一次资源导入或 Bundle 构建可能建立新基线，后续未修改资源的构建应保持稳定。

## 导入操作

1. 在工作台“导入”页选择 PS2UI Package 根目录，不要选择 `sprites`。
2. 设置图片、Prefab 和公共资源目录。
3. 检查画板预览、警告、资源复用与九宫契约。
4. 点击开始导入。
5. 每个可见画板生成相应 Prefab；会被重导覆盖的 Generated 内容不要直接挂业务脚本，业务逻辑放在外壳 Prefab。

## 字体与排版

Package 已包含字体身份和排版参数，不要求额外字体 JSON。项目中已有精确映射时自动使用 TMP Font Asset；未绑定时仍生成完整 Prefab，保留文字、字号、行高、字距、对齐和矩形，并在工作台“字体”页进入待绑定清单。

字体页可绑定 TMP 字体、材质样式并刷新已生成 Prefab。插件不复制或分发字体二进制。

## 资源复用与构建稳定性

- 解码 RGBA 像素、尺寸与九宫契约完全一致时复用已有 Sprite。
- 颜色、Alpha、轮廓或九宫边界不同的资源不会强制合并。
- 显式导入流程只在设置确实变化时执行 `SaveAndReimport`。
- 插件不注册全局 `OnPreprocessTexture`，不会参与无关图片刷新、SpriteAtlas 重建或 YooAsset/SBP Bundle 构建。
- 内容未变化的重复导入不会重写 PNG。

## 查看版本

- 打开 `Window -> Package Manager`，选择 `PS2Unity UI Importer` 查看版本。
- 或查看 `Packages/com.psd2unity.uiimport/package.json` 的 `version`。

## 升级与卸载

- UPM：在 Package Manager 移除旧版本，再按新版 `package.json` 安装；本地磁盘包也可直接替换后让 Unity 刷新。
- Unitypackage：完整导入新版，轻量引导器会清理旧版遗留的全局纹理后处理器。
- 卸载：优先在 Package Manager 中 Remove；传统安装需要先确认没有业务代码依赖插件程序集，再删除对应插件目录。

## 校验下载

```powershell
Get-FileHash .\PS2Unity-Unity-UPM-2.1.1.zip -Algorithm SHA256
Get-FileHash .\PS2Unity-Unity-2.1.1.unitypackage -Algorithm SHA256
```

输出应与 `PS2Unity-2.1.1-SHA256.txt` 一致。
