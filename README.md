# 玩啥 · 小游戏集合平台

> 无聊的时候，玩啥！

轻量级小游戏集合平台：首页推荐、游戏大厅（分类筛选 / 搜索 / 排序）、游戏详情、本地排行榜与个人中心。第一款接入游戏为《梳毛大师》。

**在线地址**：https://wansha.cc.cd （备用：https://wansha-cj9.pages.dev）

## 技术形态

- 单文件应用：全部代码在 `index.html`（原生 HTML/CSS/JS，无框架、无构建步骤、无外部资源）
- 托管：Cloudflare Pages（项目名 `wansha`），仓库根目录即站点根目录
- 数据：游客模式，全部存浏览器 `localStorage`（前缀 `wansha_`，按游戏 ID 隔离）；云端同步（Supabase）为第二阶段预留

## 本地运行

直接用浏览器打开 `index.html`，或任意静态服务器托管本目录即可。

## 游戏接入

平台由 `index.html` 内的 `GAMES` 注册表驱动：新增游戏只需在注册表中增加一条配置（id / 名称 / 分类 / 封面 / 描述 / 地址 / 状态 / 推荐 / 热门 / 权重 / 上线日期），首页、大厅、搜索、详情自动生效，无需改动平台核心代码。

游戏以 iframe 形式在平台内启动。游戏结束时可向平台回传成绩（跨域 `postMessage` 协议，平台校验来源域名后才记录）：

```js
window.parent.postMessage(
  { type: 'wansha:result', game: '<游戏ID>', score: 1234, rounds: 3, combo: 9 },
  'https://wansha.cc.cd'
);
```

## 部署

将本仓库根目录（含 `index.html`）作为静态站点部署到 Cloudflare Pages 即可；或在 Pages 控制台把项目连接到本仓库，推送后自动部署。
