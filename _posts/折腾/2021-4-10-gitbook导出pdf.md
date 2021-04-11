---
date: 2021-4-10 21:31
tag: 折腾
key: play
status: public
title: gitbook导出pdf
---

整理Chrome里的书签时，发现了一本[xv6中文文档](https://th0ar.gitbooks.io/xv6-chinese/content/)gitbook，刚好最近想研究一下这方面的内容，于是就看了一下。但是我不太习惯使用浏览器在网络环境上看书，打算下载下来，却没有发现pdf版。Google之，发现可以把GitHub项目下载下来，[使用gitbook生成pdf](https://github.com/GitbookIO/gitbook/blob/master/docs/ebook.md)。

1. 使用git把项目下载下来

   ```powersehll
   git clone https://github.com/ranxian/xv6-chinese
   ```

2. 下载node.js。推荐使用[nvs](https://github.com/jasongin/nvs/releases)下载，nvs是一个node.js版本管理工具，下载、切换及切换node.js版本非常方便。

   1. 下载nvs并安装

   2. 打开powershell，输入`nvs`，按照提示下载一个node.js版本，这里我们下载一个`10.X`版本，否则安装gitbook会报错

   3. 再次输入nvs，选择一个node.js版本。这一步非常重要，而且在每次重新打开powershell后，都要使用nvs选择一个node.js版本，否则npm命令不能使用

      ```
      PS C:\> nvs
      .------------------------------------------.
      | Select a version                         |
      +------------------------------------------+
      |  a) node/15.14.0/x64                     |
      |  b) node/15.10.0/x64                     |
      | [c] node/10.24.0/x64 (Dubnium) [current] |
      |                                          |
      |  ,) Download another version             |
      |  .) Don't use any version                |
      '------------------------------------------'
      Type a hotkey or use Down/Up arrows then Enter to choose an item.
      ```

3. 使用npm下载gitbook-cli

   ```powershell
   npm install gitbook-cli -g
   gitbook -V
   ```

4. 安装[calibre-ebook](https://calibre-ebook.com/download_windows)，这是pdf转换器，如果没有，在使用`gitbook pdf`时，会报错

   ```powershell
   EbookError: Error during ebook generation: 'ebook-convert' 
   ```

5. 切换到下载下来的GitHub项目目录，使用以下命令进行转换，在目录下生成一个book.pdf

   ```powershell
   gitbook pdf
   ```

   如果需要封面，可以在目录下放一个cover.jpg
   
6. 若需要自定义格式，可在项目目录下建一个 `styles` 文件夹，里面创建一个`pdf.css`，把样式写进去。我使用Typora的[`github.css`](https://theme.typora.io/theme/Github/)主题，把 `@font-face` 属性删除。



我导出的pdf左右边距非常大，试了很多方法都搞不定，只能放弃。于是我选择使用Typora导出单个pdf再合并的方法来把gitbook转换成pdf🤣（留下没有技术的泪水 ( ╯□╰ )

