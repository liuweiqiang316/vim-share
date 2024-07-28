---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides, markdown enabled
title: 在vscode中使用vim

# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

# 在vscode中使用vim

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
</div>

<!--
皮一下：安装vim插件，分享结束
-->

---
transition: fade-out
preload: false
---

# vim是什么

<div class="quote">
    <span>
        Vim is a <span v-mark="{at: 2, color: 'rgb(248 113 113 )'}">highly configurable</span> text editor built to make creating and
        changing any kind of text very <span v-mark="{at: 1, color: 'rgb(248 113 113 )'}">efficient</span>.
    </span>
    <span class="quote-link">
        ---<a src="https://www.vim.org">vim.org</a>
    </span>
</div>

<!--
两部分
1. 高效
2. 可配置

本次不讲高度可配置，只讲如何高效编辑

-->

---
layout: full
---

# 场景


````md magic-move
```ts

// 给一个函数包裹一个节流函数
export function fetchLogin(userName: string, password: string) {
  return request<Api.Auth.LoginToken>({
    url: '/auth/login',
    method: 'post',
    data: {
      userName,
      password
    }
  });
}

```
```ts

export const fetchLogin = throttle((userName: string, password: string) => {
  return ({
    url: '/auth/login',
    method: 'post',
    data: {
      userName,
      password
    }
  });
})

```
````

<div v-click>如果有一百个函数需要修改呢？</div>


<!--

1. 给请求函数包裹一个节流函数
2. 进阶：@@ 重复执行
3. 进阶：一次给整个文件的函数包裹节流


先看基础操作，再根据基础操作组合进行宏（marco）

-->

---
layout: image-right
image: https://cover.sli.dev?t=basic
---

# vim基础

<div class="custom-two-cols" >
 <ul>
  <li>1. 保存、退出</li>
  <li>2. 移动</li>
  <li>3. 编辑</li>
  <li>4. 删除</li>
 </ul>
 <ul>
  <li>5. 撤销</li>
  <li>6. 修订替换</li>
  <li>7. 查找替换</li>
  <li>8. 跳转</li>
 </ul>
</div>


---
layout: image-left
image: https://cover.sli.dev?t=save
---

# 保存、退出
<code>:w</code> 保存<br />
<code>:w!</code> 强制保存<br />
<code>:q</code> 退出<br />
<code>:wq</code> 保存并退出<br />
<code>:q!</code> 保存并退出<br />
<code>ZZ</code>保存并退出<br />

---
layout: image-right
image: https://cover.sli.dev?t=move
---
# 移动
<code>hjkl</code><br />
<code>w</code>下一个单词开头<br />
<code>b</code>前一个单词开头<br />
<code>e</code>下一个单词结尾<br />
<code>0</code>行首<br />
<code>$</code>行尾<br />
<code>^</code>第一个非空字符<br />
<code>%</code>括号匹配<br />
<code>*</code>向下匹配光标下单词<br />
<code>#</code>向上匹配光标下单词<br />
<code>gg</code>首行<br />
<code>G</code>最后一行<br />
<code>nG</code>3G<br />

---
layout: image-left
image: https://cover.sli.dev?t=edit
---
## 编辑
<code>y</code>yank<br />
<code>i</code>insert<br />
<code>a</code>append<br />
<code>o</code>下一行插入<br />
<code>I</code>行首插入<br />
<code>A</code>行尾插入<br />
<code>O</code>上一行插入<br />
<code>p</code>put<br />
<code>r</code>replace<br />
<code>c</code>change<br />
<code>.</code>repeat<br />

## 删除
<code>d</code>delete<br />
<code>dd</code>删除当前行<br />
<code>D</code>删除到行尾<br />

## 撤销
<code>u</code>撤销 undo<br />
<code>c-r</code>反撤销 redo<br />

---
layout: two-cols
---

# 查找

<div class="custom-center">
  <div class="item">
    <code>?pattern</code>向前找
  </div>
  <div class="item">
    <code>/pattern</code>向后找
  </div>
  <div class="item">
    <code>n</code>
  </div>
  <div class="item">
    <code>N</code>
  </div>
</div>


::right::
# 替换
<div class="custom-center">
  <div class="item">
    <code>:s/old/new</code>
    <span>替换本行第一个</span>
  </div>
  <div class="item">
    <code>:s/old/new/g</code>
    替换本行所有
  </div>
  <div class="item">
    <code>:%s/old/new</code>
    <span>替换全文件所有</span>
  </div>
  <div class="item">
    <code>:%s/old/new/gc</code>
    <span>替换全文件所有,并给出确认提示</span>
  </div>
  <div class="item">
    <code>:#,#s/old/new/g</code>
    <span>#,#是要更改范围</span>
  </div>
</div>

---
layout: image-right
image: https://cover.sli.dev?t=jump
---
# 跳转
<div class="custom-center">
  <div class="item">
    <code>c-i</code>
    <span>向前</span>
  </div>
  <div class="item">
    <code>c-o</code>
    <span>向后</span>
  </div>
  <div class="item">
    <code>gd</code>
    <span>跳转定义</span>
  </div>
  <div class="item">
    <code>gi</code>
    <span>上一次编辑的地方，并编辑</span>
  </div>
</div>

---
layout: image-left
image: https://cover.sli.dev?t=marco
---

# 宏
<div class="custom-center">
  <div v-click>
  宏命令的基本语法如下：

  ```vim
  qa                     开始记录动作到寄存器 a
  q (while recording)    停止记录
  ```
  </div>
  <div v-click>
  你可以使用小写字母 （a-z）去存储宏命令。并通过如下的命令去调用：
  
  ```vim
  @a    Execute macro from register a
  @@    Execute the last executed macros
  ```
  </div>
  <div v-click>
    <Link to="3">如何给函数包裹一个节流?</Link>
  </div>
</div>

<!-- 
1. margo 基本用法
2. 处理最开始提出的问题: 包裹函数
-->

--- 
layout: end
--- 

# 谢谢
<!-- 
疑问解答
-->