# Iris.to 中文化

一个把 [iris.to](https://iris.to/) 界面翻译成简体中文的油猴脚本。

## 安装

1. 安装 [Tampermonkey](https://www.tampermonkey.net/)（或 Violentmonkey 等兼容扩展）。
2. 打开扩展的管理面板，新建脚本。
3. 把 `iris-zh.user.js` 的内容完整粘贴进去，保存。
4. 打开或刷新 https://iris.to/。

脚本会匹配以下域名：

- `iris.to`
- `*.iris.to`（含 `beta.iris.to`、`groups.iris.to`）
- `iris-client.irisapp.workers.dev`
- `iris-groups.irisapp.workers.dev`

## 构建

```bash
npm install
npm run build
```

生成 `iris-zh.min.js`，压缩时会保留油猴脚本的元数据头。

## 已覆盖的范围

- 导航、信息流、通知、个人资料、设置、钱包、聊天、群组、市场、举报等页面。
- 按钮、菜单、开关说明、输入框占位符、图标提示（`title` / `aria-label` / `alt`）。
- 页面标题，以及 `Search: xxx`、`3 followers`、`2h`、`Saved relays (6)` 这类带变量的文案。
- SPA 路由切换和新加载的内容，通过 `MutationObserver` 自动处理。

## 实现方式

- 使用精确匹配的词典替换界面文案，默认不改写用户生成内容。
- 帖子正文、简介、聊天内容等区域通过 `.prose`、`pre-wrap`、输入框、代码块等选择器跳过。
- 匹配不到精确词条时使用少量正则规则处理数量、时间等动态文本。
- 大小写不敏感回退，兼容页面用 CSS 显示成全大写的标题。

## 已知限制

- Iris.to 更新文案或调整 DOM 结构后，可能需要同步补充词典或修改选择器。
- 用户生成内容如果与界面词条完全同名，且不在跳过区域内，理论上可能被误翻。
- 相对时间只覆盖 `s` / `m` / `h` / `d` / `w` / `y` 短格式。
- 部分网络页、钱包页文案是按截图补充的，未在所有状态下逐项验证。

## 维护

词典和规则都在 `iris-zh.user.js` 顶部的 `DICT` 与 `RULES` 中，按页面分组。新增词条时保持原文大小写和标点一致即可。
