# LaTeX Template Reference — Hướng dẫn xuất đề/solution sang LaTeX

> Phân tích từ template AIO2025 (`Quiz-POS-Tagging_solution.tex`).
> Agent đọc file này khi cần xuất LaTeX thay vì Markdown.

---

## 1. Preamble (copy nguyên, chỉ thay title/author)

```latex
\documentclass[12pt]{article}
\usepackage[utf8]{vietnam}
\usepackage[vietnamese=nohyphenation]{hyphsubst}
\usepackage[vietnamese]{babel}
\AtBeginDocument{\shorthandoff{"}}
\usepackage[table,dvipsnames]{xcolor}
\usepackage{booktabs}
\usepackage{ragged2e}
\usepackage{xcolor}
\usepackage{caption}
\usepackage{subcaption}
\usepackage{float}
\usepackage{amsmath,amssymb,amsfonts}
\usepackage{graphicx}
\usepackage{adjustbox}
\usepackage{multicol}
\usepackage[inline,shortlabels]{enumitem}
\usepackage{forest}
\usepackage{setspace}
\usepackage{longtable}
\usepackage{bm}
\usepackage[inkscapelatex=false]{svg}
\usepackage{titling}
\usepackage{tabularx}
\usepackage{makecell}
\usepackage[T1]{fontenc}
\usepackage{lmodern,mathrsfs}
\usepackage{hyperref}
\usepackage{xparse}
\usepackage[most]{tcolorbox}
\usepackage{svg}
\usepackage{tikz}
\usetikzlibrary{positioning, arrows.meta, shapes.geometric}
\usepackage{fontawesome5}
\usepackage{vipythonhighlight}
\usepackage{listings}
\usepackage{tvietlistings}
```

> **Lưu ý:** Packages `vipythonhighlight` và `tvietlistings` là custom packages của AIO. Nếu không có, thay bằng `listings` thông thường.

---

## 2. Các pattern chính

### 2a. Câu hỏi (Text)

```latex
\noindent \textbf{Câu X.} [Mô tả bối cảnh/lý thuyết ngắn gọn]

[Nội dung câu hỏi]

\begin{enumerate}[(a)]
    \item Đáp án A
    \item Đáp án B
    \item Đáp án C
    \item Đáp án D
\end{enumerate}
\noindent \textbf{Đáp án: X}
```

### 2b. Hình ảnh / Sơ đồ minh họa

**SVG (ưu tiên):**
```latex
\begin{center}
    \includesvg[width=0.9\linewidth]{Figures/hinh_1.svg}
\end{center}
```

**PNG/JPG:**
```latex
\begin{center}
    \includegraphics[width=0.9\linewidth]{Figures/hinh_1.png}
\end{center}
```

**TikZ (vẽ trực tiếp, ví dụ sơ đồ kiến trúc):**
```latex
\begin{center}
\begin{tikzpicture}[node distance=1.5cm, >=Stealth]
    \node[draw, rounded corners] (input) {Input 4×4×3};
    \node[draw, rounded corners, right=of input] (dw) {DW Conv 3×3};
    \node[draw, rounded corners, right=of dw] (pw) {PW Conv 1×1};
    \node[draw, rounded corners, right=of pw] (output) {Output 2×2×2};
    \draw[->] (input) -- (dw);
    \draw[->] (dw) -- (pw);
    \draw[->] (pw) -- (output);
\end{tikzpicture}
\end{center}
```

### 2c. Công thức toán

**Inline:**
```latex
Score: $s_j = \frac{q^\top k_j}{\sqrt{d_k}}$
```

**Display (căn giữa):**
```latex
\[
o = \sum_j \alpha_j v_j = 0.67 \begin{bmatrix}1\\0\end{bmatrix} + 0.33 \begin{bmatrix}0\\2\end{bmatrix}
\]
```

**Ma trận:**
```latex
\[
K_{DW} = \begin{bmatrix} 1 & 0 & 1 \\ 0 & 1 & 0 \\ 1 & 0 & 1 \end{bmatrix}
\]
```

### 2d. Code Python

```latex
\begin{aivncodebox}
\begin{lstlisting}[language=Python]
import numpy as np
ch0 = np.array([[1,0,2,1],[0,1,0,2],[1,2,1,0],[0,1,0,1]])
result = conv2d(ch0, kernel)
print(result)
\end{lstlisting}
\end{aivncodebox}
```

> **Nếu không có package `aivncodebox`**, dùng `lstlisting` thuần:
> ```latex
> \begin{lstlisting}[language=Python]
> # code here
> \end{lstlisting}
> ```

### 2e. Solution box (giải thích đáp án)

```latex
\begin{guidelinebox}
\textbf{Phân tích chi tiết:}
\begin{itemize}
    \item \textbf{Choice A sai:} vì...
    \item \textbf{Choice B đúng:} vì...
\end{itemize}

Tính toán:
\[
\bar{\ell} = \frac{2.00 + 0.36 + 0.51}{3} \approx 0.96
\]
\end{guidelinebox}
```

> **Nếu không có `guidelinebox`**, định nghĩa trong preamble:
> ```latex
> \newtcolorbox{guidelinebox}[2][]{
>     enhanced, colback=yellow!10!white, colframe=orange!90!black,
>     boxrule=1pt, arc=4pt, fonttitle=\bfseries,
>     attach boxed title to top left={yshift=-2mm, xshift=3mm},
>     colbacktitle=orange!90!black, coltitle=white,
>     title=\faLightbulb\hspace{1mm} #2, #1
> }
> ```

### 2f. Bảng

```latex
\begin{center}
\begin{tabular}{|c|l|c|}
\hline
\textbf{Vị trí} & \textbf{Phép tính} & \textbf{Kết quả} \\
\hline
(0,0) & $1\cdot1 + 0\cdot0 + 2\cdot1 + \ldots$ & \textbf{6} \\
(0,1) & $0\cdot1 + 2\cdot0 + 1\cdot1 + \ldots$ & \textbf{3} \\
\hline
\end{tabular}
\end{center}
```

### 2g. Lưu ý / Note box

```latex
\begin{notebox}
Thứ tự DW và PW có thể thay đổi tùy mục đích thiết kế.
\end{notebox}
```

### 2h. Ngắt trang

```latex
\newpage
```

---

## 3. Cấu trúc file hoàn chỉnh

```latex
% === PREAMBLE (copy từ template) ===
\documentclass[12pt]{article}
% ... packages ...

% === TITLE ===
\title{
  \begin{center}
    \Large {AI VIET NAM COURSE 2025}\\[.5ex]
    \Huge \textbf{[Tên chủ đề] - Quiz Solution}\\[.5ex]
  \end{center}
}
\author{[Tên tác giả]}
\date{}

% === DOCUMENT ===
\begin{document}
\maketitle

\section{Đề bài}

% Câu 1
\noindent \textbf{Câu 1.} [Bối cảnh]
% ... hình, công thức, đáp án ...
\begin{guidelinebox}
% ... solution ...
\end{guidelinebox}
\newpage

% Câu 2
% ...

\end{document}
```

---

## 4. Checklist khi xuất LaTeX

- [ ] Preamble có đủ packages?
- [ ] Tiếng Việt: dùng `\usepackage[utf8]{vietnam}` + `babel`
- [ ] Hình ảnh: file tồn tại trong thư mục `Figures/`?
- [ ] Công thức: dùng `\[...\]` cho display, `$...$` cho inline
- [ ] Code Python: nằm trong `aivncodebox` hoặc `lstlisting`
- [ ] Đáp án trắc nghiệm: dùng `\begin{enumerate}[(a)]`
- [ ] Solution: nằm trong `guidelinebox`
- [ ] Mỗi câu kết thúc bằng `\newpage`
- [ ] Ký tự đặc biệt: escape `_`, `%`, `&`, `#`, `$`, `{`, `}`
