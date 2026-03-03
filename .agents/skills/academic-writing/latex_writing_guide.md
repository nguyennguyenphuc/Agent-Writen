# LaTeX Writing Guide — Hướng dẫn xuất bài viết sang LaTeX

> Hướng dẫn format LaTeX cho bài viết học thuật theo chuẩn AIO2025.
> Rút từ `Guide_Latex.pdf` và `Guide_Latex-2.pdf`.
> Agent đọc file này khi cần xuất LaTeX.

---

## 1. Preamble chuẩn

```latex
\documentclass[12pt]{article}
\usepackage[utf8]{vietnam}
\usepackage[vietnamese=nohyphenation]{hyphsubst}
\usepackage[vietnamese]{babel}
\AtBeginDocument{\shorthandoff{"}}
\usepackage[table,dvipsnames]{xcolor}
\usepackage{booktabs}
\usepackage{ragged2e}
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
\usepackage{listings}
```

> **Lưu ý:** Packages `vipythonhighlight` và `tvietlistings` là custom packages của AIO. Nếu có sẵn, thêm:
> ```latex
> \usepackage{vipythonhighlight}
> \usepackage{tvietlistings}
> ```

---

## 2. Cấu trúc (Section / Heading)

```latex
\section{Chương 1}        % I.
\subsection{Mục 1}        % I.1.
\subsubsection{Mục nhỏ 1} % I.1.1.
```

- Các đoạn văn **không thụt lề**: thêm `\noindent` trước văn bản
- Mục lục: `\tableofcontents`

---

## 3. Hình ảnh (3 kiểu)

### Kiểu 1: Caption ngắn, hộp màu AIO (figurecardcaptionaivn)

Dùng khi: hình minh họa quan trọng, cần gây ấn tượng.

```latex
\begin{figure}[H]
\centering
\includegraphics[width=0.9\linewidth]{Figures/hinh_1.pdf}
\begin{minipage}{0.75\linewidth}
\begin{figurecardcaptionaivn}
\centering
\capfig{Minh họa bài toán QA sử dụng chatbot và AI Agent}
\label{fig:toolcalling}
\end{figurecardcaptionaivn}
\end{minipage}
\end{figure}
```

### Kiểu 2: Caption dài, viền màu trên nền sáng (figurecaptionmodern)

Dùng khi: hình đã có nhiều màu sắc, cần caption tối giản.

```latex
\begin{figure}[H]
\centering
\includegraphics[width=0.9\linewidth]{Figures/hinh_2.pdf}
\begin{minipage}{0.85\linewidth}
\begin{figurecaptionmodern}
\raggedright
\capfig{Minh hoạ quy trình xử lý và phản hồi từ Llama3.1 với công cụ tìm kiếm.
Với đường màu đỏ là phản hồi khi không sử dụng công cụ và ngược lại cho đường màu đen.}
\label{fig:pipeline}
\end{figurecaptionmodern}
\end{minipage}
\end{figure}
```

### Kiểu 3: Caption truyền thống (đơn giản)

Dùng khi: cần format đơn giản, theo chuẩn xuất bản.

```latex
\begin{figure}[H]
\centering
\includegraphics[width=0.9\linewidth]{Figures/hinh_3.pdf}
\caption{Minh hoạ quy trình xử lý.}
\label{fig:simple}
\end{figure}
```

### Quy tắc chung cho hình:

- Caption ngắn (1 dòng) → `\centering`
- Caption dài (≥ 2 dòng) → `\raggedright`
- Format: `.pdf` hoặc `.svg` (ưu tiên) hoặc `.png`
- Với SVG: dùng `\includesvg[width=0.9\linewidth]{Figures/hinh.svg}`

---

## 4. Bảng (4 kiểu)

### Kiểu 1: Bảng đơn giản + caption căn giữa

```latex
\begin{center}
\begin{minipage}{0.7\linewidth}
\refstepcounter{table}
\begin{tcolorbox}[
  colframe=aivnGreen, colbacktitle=aivnGreen,
  colback=white, boxrule=1pt,
  title=\textbf{Table \thetable: Evaluation results},
  fonttitle=\bfseries\color{white},
  halign title=center, arc=5pt
]
\renewcommand{\arraystretch}{1.5}
\begin{tabularx}{\linewidth}{>{\centering\arraybackslash}m{3cm} X c}
\toprule
\textbf{Algorithm} & \textbf{Accuracy (\%)} & \textbf{Time (s)} \\
\midrule
Model A & 92.3 & 1.45 \\
Model B & 89.7 & 1.32 \\
\bottomrule
\end{tabularx}
\end{tcolorbox}
\end{minipage}
\end{center}
```

### Kiểu 2: Bảng + đoạn văn song song

```latex
\begin{center}
\begin{minipage}[t]{0.48\linewidth}
Bảng bên trình bày kết quả đánh giá hiệu suất của ba mô hình...
\end{minipage}
\hfill
\begin{minipage}[t]{0.48\linewidth}
% Bảng ở đây
\end{minipage}
\end{center}
```

### Kiểu 3: Header 2 dòng

Dùng `\makecell` cho header nhiều dòng:

```latex
\makecell{Tên mô hình \\ (Model Name)} & \makecell{Độ chính xác \\ (Accuracy \%)}
```

### Kiểu 4: Bảng với caption dài

Caption dài → dùng `\raggedright`:

```latex
\begin{minipage}{0.75\linewidth}
\begin{captionbelowboxaivn}
\raggedright
Table \thetable: Evaluation results across various model configurations showing accuracy...
\end{captionbelowboxaivn}
\end{minipage}
```

---

## 5. Code block (aivncodebox)

### Không có tiêu đề:

```latex
\begin{aivncodebox}[]
\begin{lstlisting}[language=Python]
def factorial(n):
    if n <= 1:
        return 1
    else:
        return n * factorial(n - 1)
\end{lstlisting}
\end{aivncodebox}
```

### Có tiêu đề:

```latex
\begin{aivncodebox}[student\_management.py]
\begin{lstlisting}[language=Python]
# Step 1: Declare variables
student_name = "An"
math_score = 8.5
\end{lstlisting}
\end{aivncodebox}
```

### Định nghĩa aivncodebox (nếu chưa có):

```latex
\newtcolorbox{aivncodebox}[1][]{
  enhanced,
  colback=gray!10,
  colframe=aivnGreen,
  coltitle=white,
  colbacktitle=aivnGreen,
  title=#1,
  fonttitle=\bfseries\sffamily,
  listing only,
  listing engine=listings,
  listing options={
    language=Python,
    basicstyle=\ttfamily\footnotesize,
    commentstyle=\color{ForestGreen},
    keywordstyle=\color{blue},
    stringstyle=\color{red!70!black},
    numbers=left,
    numberstyle=\tiny\color{gray},
    numbersep=5pt,
    breaklines=true,
    showstringspaces=false,
    xleftmargin=1.5em,
    framexleftmargin=1.5em
  }
}
```

---

## 6. Hộp thông tin

### Hộp Khái niệm:

```latex
\begin{conceptbox}{Tên khái niệm}
Ý tưởng: Sử dụng hộp này để định nghĩa hoặc giải thích một khái niệm quan trọng.
\end{conceptbox}
```

### Hộp Tip / Mẹo:

```latex
\begin{tipbox}{Mẹo}
Sử dụng hộp này để đưa ra các mẹo, hướng dẫn hoặc quy tắc thực hành.
\begin{itemize}
    \item Cho bài toán phân loại: $m \approx \sqrt{n}$.
    \item Cho bài toán hồi quy: $m \approx n/3$.
\end{itemize}
\end{tipbox}
```

### Hộp Lưu ý:

```latex
\begin{notebox}
Đây là hộp lưu ý, dùng để nhấn mạnh một thông tin quan trọng hoặc cảnh báo.
\end{notebox}
```

### Hộp Yêu cầu:

```latex
\begin{requirementbox}{Yêu cầu}
Xây dựng Regression Tree với độ sâu bằng 2 từ dữ liệu trên...
\end{requirementbox}
```

---

## 7. Bảng ký hiệu toán học

```latex
\begin{center}
\begin{tcolorbox}[
  colframe=aivnGreen, colbacktitle=aivnGreen,
  title=\textbf{Bảng Ký hiệu Toán học},
  fonttitle=\bfseries\color{white}
]
\begin{tabularx}{\linewidth}{c X}
\toprule
\textbf{Ký hiệu} & \textbf{Ý nghĩa} \\
\midrule
$B$ & Siêu tham số, tổng số cây quyết định \\
$N$ & Tổng số điểm dữ liệu trong tập huấn luyện \\
\bottomrule
\end{tabularx}
\end{tcolorbox}
\end{center}
```

---

## 8. Layout 2 cột

```latex
\begin{minipage}[t]{0.48\linewidth}
% Nội dung cột trái (hình, bảng)
\end{minipage}
\hfill
\begin{minipage}[t]{0.48\linewidth}
% Nội dung cột phải (giải thích, chú thích)
\end{minipage}
```

---

## 9. Màu sắc

6 cặp màu đã định nghĩa sẵn, thay thế `aivnGreen`:

| Cặp | Accent (header/viền) | Row (nền dòng chẵn) |
|:---:|:---------------------|:--------------------|
| 1 | `pair1Accent` | `pair1Row` |
| 2 | `pair2Accent` | `pair2Row` |
| 3 | `pair3Accent` | `pair3Row` |
| 4 | `pair4Accent` | `pair4Row` |
| 5 | `pair5Accent` | `pair5Row` |
| 6 | `pair6Accent` | `pair6Row` |

Cách dùng: thay `aivnGreen` → `pair1Accent` trong `colframe`, `colbacktitle`, v.v.

---

## 10. Công thức toán

**Inline:** `$s_j = \frac{q^\top k_j}{\sqrt{d_k}}$`

**Display:**
```latex
\[
o = \sum_j \alpha_j v_j
\]
```

**Ma trận:**
```latex
\[
K = \begin{bmatrix} 1 & 0 & 1 \\ 0 & 1 & 0 \\ 1 & 0 & 1 \end{bmatrix}
\]
```

---

## 11. Tài liệu tham khảo

```latex
\bibliographystyle{ieeetr}
\bibliography{references}
```

Với file `references.bib`:

```bibtex
@article{yao2023react,
  title={React: Synergizing reasoning and acting in language models},
  author={Yao, Shunyu and others},
  journal={arXiv preprint arXiv:2210.03629},
  year={2023}
}
```

---

## 12. Cấu trúc file hoàn chỉnh

```latex
% === PREAMBLE ===
\documentclass[12pt]{article}
% ... packages ...

% === TITLE ===
\title{
  \begin{center}
    \Large{AI VIET NAM COURSE 2025}\\[.5ex]
    \Huge\textbf{[Tên chủ đề]}\\[.5ex]
    \large Tutorial
  \end{center}
}
\author{[Tên tác giả]}
\date{}

% === DOCUMENT ===
\begin{document}
\maketitle
\tableofcontents
\newpage

\section{Giới thiệu}
\noindent [Bối cảnh + vấn đề]
% ... hình, giải thích ...

\section{Kiến thức nền}
\subsection{Tổng quan}
% ... nội dung ...

\section{Thực hành}
\subsection{Cài đặt}
% ... code blocks ...

\section{Câu hỏi trắc nghiệm}
% ... câu hỏi ...

\section{Tài liệu tham khảo}
\bibliographystyle{ieeetr}
\bibliography{references}

\appendix
\section{Phụ lục}
% ... datasets, hints, rubric ...

\end{document}
```

---

## 13. Checklist xuất LaTeX

- [ ] Preamble có đủ packages?
- [ ] Tiếng Việt: `\usepackage[utf8]{vietnam}` + `babel`
- [ ] `\noindent` trước mỗi đoạn văn?
- [ ] Hình: file tồn tại trong `Figures/`?
- [ ] Hình: caption đúng kiểu (ngắn → centering, dài → raggedright)?
- [ ] Bảng: `\refstepcounter{table}` cho số tự động?
- [ ] Code: trong `aivncodebox` hoặc `lstlisting`?
- [ ] Hộp thông tin: đúng loại (conceptbox/tipbox/notebox)?
- [ ] Công thức: `$...$` inline, `\[...\]` display?
- [ ] Mục lục: `\tableofcontents`?
- [ ] Tham khảo: BibTeX với `references.bib`?
- [ ] Ký tự đặc biệt: escape `_`, `%`, `&`, `#`, `$`, `{`, `}`?
- [ ] Encoding: UTF-8?
