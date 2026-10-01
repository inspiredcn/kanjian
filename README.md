# 看见 — 部署说明

一个可安装到手机主屏幕的离线网页应用（PWA）。包含"火"5 张卡片和"水"6 张卡片。
没有后端，没有打包工具，没有依赖。

## 文件

| 文件 | 作用 |
|---|---|
| `index.html` | 整个应用：首页 + 两组卡片 + 全部交互 |
| `manifest.webmanifest` | 应用名称、图标、全屏模式 |
| `sw.js` | Service Worker，装好以后完全离线可用 |
| `icon-*.png` | 主屏幕图标 |

## 部署（GitHub Pages，免费，约 10 分钟）

1. 建一个 GitHub 仓库，例如 `kanjian`。
2. 把这 6 个文件直接传到仓库根目录。
3. Settings → Pages → Source 选 `main` 分支 `/ (root)` → Save。
4. 等 1–2 分钟，拿到网址 `https://<用户名>.github.io/kanjian/`。
5. 手机浏览器打开这个网址。
   - **安卓 Chrome**：菜单 →「添加到主屏幕」/「安装应用」
   - **iPhone Safari**：分享 →「添加到主屏幕」（必须用 Safari，Chrome 不行）
6. 从主屏幕图标打开，全屏运行，**之后断网也能用**。

必须是 HTTPS，Service Worker 才会生效。GitHub Pages 自带 HTTPS。

## 改内容以后

改完 `index.html`，**必须把 `sw.js` 里的 `VERSION` 改一个新值**（例如 `kanjian-v2`），
否则手机上还是旧版本。改完重新上传，手机里的应用下次联网打开时会自动更新。

## 本地预览

```
cd 这个文件夹
python3 -m http.server 8000
```
浏览器开 `http://localhost:8000`。（`file://` 直接打开也能看，但 Service Worker 不工作。）

## 已知限制 / 下一步

- **第一次必须联网**。PWA 的离线只在安装之后生效。要在真正没网的地方分发，
  需要打包成 APK（PWABuilder 或 Capacitor，都能直接吃现在这套文件），或者做成微信小程序。
- **iOS 的 PWA 支持比安卓差**：必须用 Safari 添加，缓存可能被系统清理。
  测试对象如果用的是安卓机（农村更常见），体验会好得多。
- **目前是手动选对象**，没有摄像头识别。识别模型是下一轮的事。
- 卡片内容全部写死在 `index.html` 里。到第 3、4 个对象时应该抽成一个 JSON，
  不然每加一个对象都要改一次主文件。
