# Ubuntu LaTeX中文环境搭建

## 1  需求
在Ubuntu 20.04/gnome3桌面环境下搭建latex编辑、编译环境

## 2  latex组件准备

- 安装基础latex组件,linux平台下推荐texlive: `sudo apt install texlive` 

- 安装latex编译器xetex: `sudo apt install texlive-xetex`

- 安装中文包xeCJK，本人没有使用[CTeX](http://www.ctex.org/HomePage)作为中文支持方式 : `sudo apt install texlive-lang-chinese`

- 安装一些额外的字体(Optional): `sudo apt install texlive-fonts-extra`

- 安装vs code latex插件 LaTeX Workshop, 个人是[vs code](https://code.visualstudio.com/)重度用户，因此直接选择vs code作为Latex的编辑器

![插件安装](assets/images/tips/2_1.png)

## 3  配置LaTex Wordshop
- 打开vs code setting json文件，在vs任意窗口依次键入`F1`、`open settings`

![选择setting文件](assets/images/tips/2_2.png)

- 配置vs pdf浏览器，我选择的是默认的tab; 配置LaTeX默认编译器、recipes以及工具链


```json
    "latex-workshop.view.pdf.viewer": "tab",
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ],
        },
        {
            "name": "pdflatex",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "xe->bib->xe->xe",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdf->bib->pdf->pdf",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        }
    ],
```

## 4 测试
新建一个 temp.tex文件，键入

```latex
\documentclass{article}
\usepackage{xeCJK}

\title{Latex学习}
\author{Jubo Ge\thanks{GuLu}}
\date{\today}

\begin{document}

\maketitle

\section{知乎，发现更大的世界}
你好吗

\section{你知道吗}
$ \sum_{i=1}^n{D_i}$

\section*{Reference}

\end{document}
```
按`ctrl+alt+v`或者手动点击工具栏，查看测试效果

![最终效果](assets/images/tips/2_3.png)


## Reference

[Deepin Linux 安装和搭建LaTex环境](https://zhuanlan.zhihu.com/p/40053417)


[使用VSCode编写LaTeX](https://zhuanlan.zhihu.com/p/381780157)