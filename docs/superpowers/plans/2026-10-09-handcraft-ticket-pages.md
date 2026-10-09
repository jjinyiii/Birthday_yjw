# 花边票根手账页面 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将首页、视频页和信件页统一升级为花边票根手账风格，同时保持现有照片、视频、BGM、蛋糕和信纸弹层功能不变。

**Architecture:** 继续使用单文件 `index.html`。新增页面专属 class 和伪元素装饰，避免运行时文字替换脚本与第三方依赖。首页、视频页、信件页各自拥有独立装饰层，路由逻辑和交互逻辑只做必要的元素引用调整。

**Tech Stack:** HTML5、CSS、原生 JavaScript、现有 Google Fonts、现有本地媒体资源。

---

### Task 1: 清理页面结构并建立三页装饰边界

**Files:**
- Modify: `D:/Birthday_yjw/index.html`（页面 CSS 与三个 `data-view` section）

- [ ] **Step 1: 为三个页面添加专属根 class**

在现有 section 上保持 `data-view`，补充 class：

```html
<section id="view-home" class="view home-page ticket-page ticket-home" data-view="home">
<section id="view-video" class="view ticket-page ticket-video" data-view="video">
<section id="view-letter" class="view ticket-page ticket-letter" data-view="letter">
```

- [ ] **Step 2: 添加装饰性语义元素**

装饰只使用 `aria-hidden="true"`：

```html
<span class="ticket-edge ticket-edge-left" aria-hidden="true"></span>
<span class="ticket-edge ticket-edge-right" aria-hidden="true"></span>
<span class="page-stamp" aria-hidden="true">KEEP<br />THIS</span>
```

首页放票根编号，视频页放放映编号，信件页放邮戳；不要跨页面复用同一装饰文案。

- [ ] **Step 3: 删除任何用于全页面文字替换的临时脚本**

保留单一初始化脚本和现有路由、BGM、视频、蛋糕、信件逻辑，确保页面文字全部由 HTML 静态提供。

- [ ] **Step 4: 静态检查页面结构**

运行：

```powershell
node -e "const fs=require('fs'); const html=fs.readFileSync('D:/Birthday_yjw/index.html','utf8'); let pos=0,n=0; while((pos=html.indexOf('<script',pos))>=0){const start=html.indexOf('>',pos)+1; const end=html.indexOf('</script>',start); if(start<=0||end<0) throw Error('script boundary'); new Function(html.slice(start,end)); n++; pos=end+9;} console.log(JSON.stringify({scriptCount:n,parse:'ok'}));"
```

Expected: `parse:"ok"`。

### Task 2: 实现首页“生日收藏册”装饰

**Files:**
- Modify: `D:/Birthday_yjw/index.html`（`.home-page`、`.photo-wall`、`.cake-card` 样式）

- [ ] **Step 1: 添加首页票根和花边样式**

加入以下 CSS 规则，装饰不改变布局尺寸：

```css
.ticket-home { position: relative; }
.ticket-home::before { content: "BIRTHDAY ARCHIVE · 01"; position:absolute; top:4px; right:18px; color:#a47d75; font:0.82rem "Caveat", cursive; letter-spacing:.12em; transform:rotate(3deg); }
.ticket-home::after { content:""; position:absolute; inset:10px 6px auto; height:1px; border-top:1px dashed #d6bdb4; pointer-events:none; }
.ticket-home .photo-wall { border:1px dashed #d6bdb4; border-radius:3px; background:rgba(255,253,248,.4); }
.ticket-home .cake-card { border-style:dashed; }
```

- [ ] **Step 2: 保留照片四格和蛋糕交互**

不修改图片路径、caption 文案、`data-candle`、`lightCandleButton` 或 confetti 调用；仅增加装饰节点和 `aria-hidden` 标记。

- [ ] **Step 3: 检查首页移动端**

使用 390px 截图确认票根文字不压住标题，照片、蛋糕和按钮没有横向溢出。

### Task 3: 实现视频页“影院票根”装饰

**Files:**
- Modify: `D:/Birthday_yjw/index.html`（`.ticket-video` 与视频结构）

- [ ] **Step 1: 将视频页包装为影院票根**

在 `.video-layout` 内的电视外壳增加：

```html
<span class="screening-ticket" aria-hidden="true">SCREENING 02 · KEEP THIS DAY</span>
```

- [ ] **Step 2: 添加撕边和齿孔样式**

```css
.ticket-video .retro-tv { position:relative; border-radius:8px 8px 14px 14px; }
.ticket-video .retro-tv::before { content:""; position:absolute; top:-10px; left:18px; right:18px; border-top:1px dashed #b98d84; }
.ticket-video .retro-tv::after { content:"○ ○ ○ ○ ○ ○"; position:absolute; top:-21px; right:18px; color:#b98d84; font-size:.62rem; letter-spacing:.2em; }
.screening-ticket { display:inline-block; margin:0 0 12px; padding:4px 10px; border:1px dashed #c99e96; color:#8d6e69; font:0.9rem "Caveat", cursive; transform:rotate(-2deg); }
```

- [ ] **Step 3: 保留唯一视频文案和相对路径**

确认视频页只显示 `你就欣赏吧，别太感动了`，`<source src="superise.mp4">` 不变，视频播放时暂停 BGM、暂停或结束后恢复 BGM 的事件监听不变。

### Task 4: 实现信件页“邮寄私信”装饰

**Files:**
- Modify: `D:/Birthday_yjw/index.html`（`.ticket-letter`、`.envelope`、`.letter-paper` 样式）

- [ ] **Step 1: 添加信封页邮戳与票根编号**

在 `.letter-stage` 内加入：

```html
<span class="mail-stamp" aria-hidden="true">POSTED<br />19</span>
<span class="mail-ticket" aria-hidden="true">LETTER 03 / FOR YOU</span>
```

- [ ] **Step 2: 为弹层信纸添加撕边和邮寄标记**

使用现有 `.letter-paper::before` 和 `.letter-paper::after` 扩展装饰，不改变滚动容器和居中弹层结构。装饰必须位于纸张边缘，不遮挡 `To 静雯`、正文或关闭按钮。

- [ ] **Step 3: 回归信件交互**

确认点击信封后弹层居中显示；正文区域独立滚动；点击遮罩、收起按钮和 Esc 都关闭；关闭后信封恢复未拆状态、焦点返回信封按钮。

### Task 5: 浏览器回归与提交前验证

**Files:**
- Test: `D:/Birthday_yjw/index.html`

- [ ] **Step 1: 运行静态检查**

```powershell
git diff --check
node -e "const fs=require('fs'); const html=fs.readFileSync('D:/Birthday_yjw/index.html','utf8'); if(!html.includes('superise.mp4')) throw Error('video source missing'); if(!html.includes('你就欣赏吧，别太感动了')) throw Error('video copy missing'); console.log('static checks ok');"
```

- [ ] **Step 2: 浏览器截图检查**

使用 Edge 无头模式分别打开 `#home`、`#video`、`#letter`，检查 1440px 和 390px：无横向溢出、票根装饰不遮挡内容、视频小电视和信纸弹层位置稳定。

- [ ] **Step 3: 手动交互回归**

验证首页点蜡烛、视频播放/BGM 联动、信封拆信/关闭/滚动，确认装饰只影响视觉不改变交互。

- [ ] **Step 4: 提交变更**

```powershell
git add index.html
git commit -m "Polish pages with distinct ticket scrapbook styling"
```
