# 联系页 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将单行页脚改为独立、可访问且响应式的三项联系方式页面。

**Architecture:** 在现有单文件静态页中替换页脚 HTML，并在同一份内联 CSS 中新增 `contact-section` 组件样式。电话和邮箱使用原生链接；微信只显示已确认的号码。无需新增依赖或 JavaScript 状态。

**Tech Stack:** HTML、CSS、现有内联 JavaScript 测试命令、浏览器本地预览。

## Global Constraints

- 仅展示电话 `186 3255 8659`、微信 `18632558659`、邮箱 `haoad2024@163.com`。
- 顶部个人网站链接保持不变，联系列表不再展示个人网站。
- 桌面端使用留白、编号、细分隔线与低对比度装饰数字；小屏隐藏装饰数字并避免联系方式溢出。
- 不引入依赖，不修改现有项目卡片、弹窗或滚动进度交互。

---

### Task 1: 建立联系页结构与样式

**Files:**
- Modify: `index.html:120-125`、`index.html:205`
- Test: `/private/tmp/portfolio-contact-page.test.js`

**Interfaces:**
- Consumes: 当前 `site-footer` 单行页脚与 CSS 变量 `--paper`、`--ink`、`--muted`、`--line`、`--yellow`、`--blue`。
- Produces: `.contact-section`、`.contact-list`、`.contact-row` 三项列表与响应式样式。

- [ ] **Step 1: 写入失败的结构测试**

```js
assert.match(html, /class="contact-section"/);
assert.match(html, /电话/);
assert.match(html, /微信/);
assert.match(html, /邮箱/);
assert.match(html, /href="tel:18632558659"/);
assert.match(html, /href="mailto:haoad2024@163.com"/);
```

- [ ] **Step 2: 运行测试并确认失败**

Run: `node /private/tmp/portfolio-contact-page.test.js`

Expected: FAIL，提示缺少 `contact-section`。

- [ ] **Step 3: 替换页脚并添加最小样式**

```html
<footer class="contact-section" aria-labelledby="contact-title">
  <p class="contact-section__eyebrow" id="contact-title">联系 / ELSEWHERE</p>
  <ol class="contact-list">
    <li class="contact-row"><span>01</span><strong>电话</strong><a href="tel:18632558659">186 3255 8659</a></li>
    <li class="contact-row"><span>02</span><strong>微信</strong><span>18632558659</span></li>
    <li class="contact-row"><span>03</span><strong>邮箱</strong><a href="mailto:haoad2024@163.com">haoad2024@163.com</a></li>
  </ol>
</footer>
```

```css
.contact-section { position:relative; min-height:92svh; }
.contact-row { border-top:1px solid var(--line); }
@media (max-width:640px) { .contact-section__number { display:none; } }
```

- [ ] **Step 4: 运行结构与语法检查**

Run: `node /private/tmp/portfolio-contact-page.test.js && git diff --check`

Expected: PASS，且无 diff 空白错误。

### Task 2: 在浏览器验证桌面与小屏布局

**Files:**
- Modify: `index.html`（仅在验证发现问题时）
- Test: `/private/tmp/portfolio-contact-page.test.js`

**Interfaces:**
- Consumes: Task 1 输出的 `contact-section`、电话与邮箱链接。
- Produces: 经渲染验证的联系人页面；桌面和小屏联系方式不溢出。

- [ ] **Step 1: 在本地启动静态服务**

Run: `python3 -m http.server 4176 --bind 127.0.0.1`

Expected: 本地页面可由 `http://127.0.0.1:4176/` 访问。

- [ ] **Step 2: 检查桌面端联系页**

使用浏览器滚动到页面底部，确认“联系 / ELSEWHERE”、三条列表、低对比度装饰数字均可见；电话和邮箱链接分别保留 `tel:` 与 `mailto:` 目标。

- [ ] **Step 3: 检查小屏断点**

将视口设为 390×844，确认联系方式单列可读、无横向滚动，装饰数字隐藏。

- [ ] **Step 4: 运行最终检查**

Run: `node /private/tmp/portfolio-contact-page.test.js && git diff --check`

Expected: PASS，浏览器控制台无与联系页相关的 warning 或 error。
