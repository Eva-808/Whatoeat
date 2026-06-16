# 今天吃什么 🍱 (Whatoeat)

一个解决「每天不知道吃什么」的个人小应用。把想吃的东西记进清单，按分类和心情标签管理，不知道吃啥时一键随机抽签。

- 仓库：https://github.com/Eva-808/Whatoeat
- 使用方式：iOS Edge 浏览器打开线上网址 → 「添加到主屏幕」，当成桌面 App 用。

---

## 文件结构

| 文件 | 作用 | 是否必需 |
|------|------|----------|
| `index.html` | 整个应用（HTML + CSS + JS 全在一个文件里），这是当前正式版 | ✅ 必需 |
| `sw.js` | Service Worker，负责离线缓存，断网也能打开 | ✅ 必需 |
| `icon.png` | 桌面图标 | ✅ 必需 |
| `今天吃什么.html` | 早期扁平草稿，已被 index.html 取代 | ⬜ 可删 |
| `README.md` | 本说明文件 | ⬜ 仅供参考 |

> 当前正式版 = `index.html`，特征：黏土质感 + 便当格子布局 + 按下弹性回弹（`--spring` 曲线 / `.pop` 类）。

---

## 功能

- **记录**：把想吃的一次性写进清单（收藏夹）。
- **维度**：按「正餐 / 零食 / 饮品 / 甜品」等分类记录，含店名 / 牌子。
- **标签**：给每项打心情标签（没胃口、想吃辣、想吃甜的…），分类和标签都可自定义新增。
- **抽签**：按分类 + 心情筛选，随机抽一个；可避开最近 3 天吃过的。
- **备份**：设置页可「下载备份 / 导入备份」（JSON 文件）。

---

## 数据存储（重要）

- 你的清单数据**存在浏览器本地（localStorage）**，不在仓库文件里，也不上传到 GitHub。
- 含义：
  - 换**电脑**不影响数据（数据在手机上）。
  - 换**手机**、清缓存、删 App 会丢数据 → 务必先用 App 内「下载备份」导出 JSON（建议存 iCloud），新设备用「导入备份」恢复。
- 待办想法：接免费云数据库（Supabase / Firebase）实现自动云同步、多设备共享，免去手动备份。尚未实现。

---

## 已修复的问题（历史）

- **新分类 / 新标签点击卡死**：原因是用了 `window.prompt()`，在微信内置浏览器和 iOS「添加到主屏幕」的独立(standalone)模式下会被屏蔽，看起来像卡住。
  - 修复：用自建的页面内输入弹窗 `uiPrompt()`（`index.html` 里的 `#modalMask` 弹窗 + `addCat()`/`addTag()`）替代 `prompt()`，全平台可用。
  - 同时 `sw.js` 缓存版本升到 `eatwhat-cache-v4`，确保新版能覆盖旧缓存。

---

## 部署 / 更新流程

1. 改完 `index.html` / `sw.js` 后，在 GitHub 仓库点 **Add file → Upload files**，拖入文件覆盖。
2. 提交时选 **Commit directly to the main branch**（个人项目无需 PR）。
3. **每次改 `index.html` 都顺手把 `sw.js` 里的缓存版本号 +1**（v4 → v5…），否则手机可能继续用旧缓存。
4. 手机更新：删掉桌面快捷方式 → Edge 里强制刷新一两次 → 重新「添加到主屏幕」。

---

## 版本管理（GitHub Tags）

- 满意的版本在仓库 **Releases → Create a new release** 打 tag（如 `v1`、`v2-便当版`）。
- 回溯：
  - 所有上传记录：`github.com/Eva-808/Whatoeat/commits/main`
  - 正式版本：`github.com/Eva-808/Whatoeat/tags`

---

## 换电脑 / 换工具（Codex、其他 AI）继续开发

1. 在 GitHub 仓库 **Code → Download ZIP** 下载（或直接 clone / 连接 GitHub）。
2. 用工具打开这个文件夹，让它读本 `README.md` 即可了解全貌。
3. **不需要保留任何聊天记录**——代码自包含、带中文注释，这个 README 记录了背景与待办。

---

## 技术备注

- 纯静态单文件应用，无构建步骤、无依赖，双击 `index.html` 即可本地预览。
- 兼容性注意：避免使用 `prompt` / `alert` / `confirm`（iOS standalone 与微信会屏蔽），统一用页面内弹窗。
