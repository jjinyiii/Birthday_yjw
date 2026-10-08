# 首页日记页拼贴 Implementation Plan

> **For agentic workers:** Execute this plan inline in the current session; the project is a single static HTML page.

**Goal:** 将首页改造成 C 方案的日记页拼贴风格，保留现有生日互动和路由行为。

**Architecture:** 继续使用 `index.html` 内联 CSS 与现有首页 HTML。新增的装饰只属于 `#view-home`，通过伪元素、贴纸节点和响应式媒体查询实现，不触及视频、信件和 BGM JavaScript。

**Tech Stack:** HTML5、CSS、原生 JavaScript（现有代码）。

---

### Task 1: 首页装饰结构

**Files:**
- Modify: `D:/Birthday_yjw/index.html` 首页 `#view-home` 标记

- [ ] 在首页标题区域加入日期索引、页面角标和手写标签节点。
- [ ] 给照片墙增加页面边角装饰容器与每张照片的记录批注。
- [ ] 给蛋糕卡片增加纸张标签节点，不改变已有 `id`、按钮或事件选择器。

### Task 2: 日记页拼贴样式

**Files:**
- Modify: `D:/Birthday_yjw/index.html` 首页相关 CSS 与移动端媒体查询

- [ ] 为首页增加纸张纹理、边角涂鸦、印章、胶带和轻微阴影。
- [ ] 保持照片、蛋糕和 CTA 的稳定尺寸，桌面错落、移动单列。
- [ ] 确保装饰不造成横向溢出或遮挡交互按钮。

### Task 3: 回归验证

**Files:**
- Test: `D:/Birthday_yjw/index.html`

- [ ] 用文本检查确认视频、信件、BGM 相关 JS 未被改动。
- [ ] 检查内联 JavaScript 语法。
- [ ] 检查本地预览返回 HTTP 200，并在 390px / 桌面尺寸复核页面布局。
