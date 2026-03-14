# LaTeX Writing Guide — Hướng dẫn xuất bài viết sang LaTeX

> Hướng dẫn format LaTeX cho bài viết học thuật theo chuẩn AIO2025.
> Cập nhật theo `template1.tex` chính thức.
> Agent đọc file này khi cần xuất LaTeX.

---

## 1. Preamble chuẩn

Sao chép nguyên khối preamble bên dưới. **KHÔNG được bỏ bớt package** trừ khi có lý do rõ ràng.

```latex
\documentclass[12pt]{article}
\usepackage[utf8]{vietnam}
\usepackage[vietnamese=nohyphenation]{hyphsubst}
\usepackage[vietnamese]{babel}
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
```

### 1.1. Biblatex (thay cho IEEEtran.bst)

> **QUAN TRỌNG:** Template dùng `biblatex` với backend `bibtex`, **KHÔNG** dùng `\bibliographystyle` + `\bibliography`.

```latex
\usepackage[
    backend=bibtex,
    style=ieee,
    maxnames=1,
    minnames=1,
    uniquelist=false,
    hyperref=false
]{biblatex}

\addbibresource{references.bib}
\DefineBibliographyStrings{english}{
  andothers = {et\addabbrvspace al\adddot}
}
\AtBeginBibliography{\selectlanguage{english}}
\AtBeginDocument{\renewcommand\refname{}}
```

### 1.2. Appendix

```latex
\usepackage[toc,page]{appendix}
\renewcommand{\appendixtocname}{Phụ lục}
\renewcommand{\appendixpagename}{Phụ lục}
```

### 1.3. Các package bổ sung

```latex
\usepackage{xurl}
\usepackage{framed}
\usepackage{fancyhdr}
\usepackage{listings}
\usepackage{tvietlistings}       % Tiếng Việt trong listings
\usepackage{epigraph}
\usepackage{titlesec}

\PassOptionsToPackage{vietnamese.licr}{babel}
\usepackage{fontawesome5}
\usepackage{vipythonhighlight}   % Custom package AIO — highlight Python
```

### 1.4. Page layout

```latex
\setlength{\headheight}{14.5pt}
\setlength{\topmargin}{-.5in}
\setlength{\textheight}{9.25in}
\setlength{\oddsidemargin}{-0.1in}
\setlength{\textwidth}{6.8in}
```

### 1.5. Mục lục (tocloft)

```latex
\usepackage{tocloft}
\renewcommand{\cfttoctitlefont}{\Huge\bfseries}
\setlength{\cftaftertoctitleskip}{2em}
\setlength{\cftbeforesubsecskip}{7pt}
\renewcommand{\cftsecleader}{\cftdotfill{\cftdotsep}}
\setlength{\cftsecnumwidth}{3em}
\renewcommand{\cftsecaftersnum}{\quad}
\renewcommand{\cftsubsecleader}{\cftdotfill{\cftdotsep}}
\setlength{\cftsubsecnumwidth}{4em}
\renewcommand{\cftsubsecaftersnum}{\quad}
\renewcommand{\cftsubsubsecleader}{\cftdotfill{\cftdotsep}}
\setlength{\cftsubsubsecnumwidth}{5em}
\renewcommand{\cftsubsubsecaftersnum}{\quad}
```

---

## 2. Định nghĩa màu sắc

### Màu chính (bắt buộc)

```latex
\definecolor{aivnGreen}{RGB}{52,79,30}
\definecolor{captiongray}{gray}{0.15}
\definecolor{captionbg}{RGB}{250,250,250}
```

### Màu cho code

```latex
\definecolor{codebackground}{RGB}{250, 250, 250}
\definecolor{codecomment}{RGB}{34, 139, 34}
\definecolor{codekeyword}{RGB}{0, 0, 255}
\definecolor{codestring}{RGB}{255, 0, 0}
\definecolor{codeidentifier}{RGB}{0, 0, 0}
\definecolor{codelinenumbers}{RGB}{90, 90, 90}

\definecolor{codegreen}{rgb}{0,0.6,0}
\definecolor{codegray}{rgb}{0.5,0.5,0.5}
\definecolor{codepurple}{rgb}{0.58,0,0.82}
\definecolor{backcolour}{rgb}{0.95,0.95,0.92}
```

---

## 3. Cấu trúc Section (Outline)

Section đánh số La Mã, subsection và subsubsection theo dạng `I.1.`, `I.1.1.`:

```latex
\renewcommand{\thesection}{\Roman{section}.}
\renewcommand{\thesubsection}{\thesection\arabic{subsection}.}
\renewcommand{\thesubsubsection}{\thesubsection\arabic{subsubsection}.}

\titleformat{\section}{\normalfont\Huge\bfseries}{\thesection}{1em}{}
\titleformat{\subsection}{\normalfont\Large\bfseries}{\thesubsection}{1em}{}
\titleformat{\subsubsection}{\normalfont\normalsize\bfseries}{\thesubsubsection}{1em}{}
```

Ví dụ sử dụng:

```latex
\section{Lý thuyết}        % → I. Lý thuyết
\subsection{Ví dụ}          % → I.1. Ví dụ
\subsubsection{Chi tiết}    % → I.1.1. Chi tiết
```

> **Lưu ý:** Không thụt lề đoạn văn → thêm `\noindent` trước mỗi đoạn.

---

## 4. Title — Logo — Author — Header

### 4.1. Logo

```latex
\setlength{\droptitle}{-8em}
\pretitle{%
    \begin{flushleft}
        \includegraphics[height=2.0cm]{Figures/logo.pdf}\par
    \end{flushleft}
}
\posttitle{}
```

### 4.2. Title

```latex
\title{%
  \begin{center}
        \Large {AI VIET NAM -- AI COURSE 2025}\\[.5ex]
        \Huge \textbf{[Tên chủ đề]}\\[.5ex]
  \end{center}
}
```

### 4.3. Author

```latex
\posttitle{
    \author{
        \begin{minipage}{\textwidth}
        \centering
        {
          Tac-Gia~1,\quad
          Tac-Gia~2,\quad
          Tac-Gia~3
        }\\
      \end{minipage}
    }
}
\date{}
```

### 4.4. Header & Footer

```latex
\pagestyle{fancy}
\fancyhf{}
\lhead{\href{https://www.facebook.com/aivietnam.edu.vn}{\textcolor{aivnGreen}{\bfseries AI VIET NAM (AIO2025)}}}
\rhead{\href{https://aivietnam.edu.vn/}{\textcolor{aivnGreen}{\bfseries aivietnam.edu.vn}}}
\fancyfoot[C]{\thepage}
\renewcommand{\headrulewidth}{1.0pt}
\renewcommand{\headrule}{{\color{aivnGreen}\hrule width\headwidth height\headrulewidth \vskip-\headrulewidth}}
```

### 4.5. Hyperref

```latex
\hypersetup{
    colorlinks=true,
    linkcolor=black,
    filecolor=magenta,
    citecolor=black,
    urlcolor=black,
    pdftitle={Overleaf Example},
    pdfpagemode=FullScreen,
}
```

---

## 5. Định nghĩa các Box (tcolorbox)

### 5.1. Caption cho bảng — `captionbelowboxaivn`

Hộp màu xanh AIO, chữ trắng, dùng làm caption phía dưới bảng.

```latex
\newtcolorbox{captionbelowboxaivn}{
    colback=aivnGreen,
    colframe=aivnGreen,
    boxrule=0pt,
    arc=4pt,
    left=4pt, right=4pt, top=2pt, bottom=2pt,
    width=\linewidth,
    boxsep=4pt,
    halign=center,
    fontupper=\color{white}
}
```

### 5.2. Caption cho hình — `figurecardcaptionaivn`

Hộp xanh AIO full-width, chữ trắng nhỏ, dùng cho hình ảnh quan trọng.

```latex
\newtcolorbox{figurecardcaptionaivn}{
  colback=aivnGreen,
  colframe=aivnGreen,
  boxrule=0pt,
  arc=2pt,
  top=3pt, bottom=3pt, left=10pt, right=10pt,
  width=\linewidth,
  fontupper=\small\fontsize{11pt}{13pt}\selectfont\color{white}
}
```

### 5.3. Caption hiện đại cho hình — `figurecaptionmodern`

Nền sáng, viền xanh AIO, chữ đen, dùng khi hình đã nhiều màu.

```latex
\newtcolorbox{figurecaptionmodern}{
  colback=captionbg,
  colframe=aivnGreen,
  boxrule=1.5pt,
  bottomrule=2pt,
  toptitle=0pt, bottomtitle=0pt,
  left=6pt, right=6pt, top=4pt, bottom=2pt,
  width=\linewidth,
  fontupper=\small\fontsize{11pt}{13pt}\selectfont\color{black},
  halign=center
}
```

### 5.4. Counter tự đếm hình — `\capfig`

```latex
\newcounter{hinh}
\newcommand{\capfig}[1]{%
  \refstepcounter{hinh}%
  Hình~\thehinh:~#1
}
```

### 5.5. Code block — `aivncodebox`

```latex
\newtcolorbox{aivncodebox}[1][]{%
  enhanced,
  colback=gray!10,
  colframe=aivnGreen,
  coltitle=white,
  colbacktitle=aivnGreen,
  title=#1,
  fonttitle=\bfseries\sffamily,
  listing only,
  breakable,
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

### 5.6. Hộp Khái niệm — `conceptbox`

Nền xanh nhạt, viền xanh AIO, tiêu đề dán phía trên trái.

```latex
\newtcolorbox{conceptbox}[2][]{
    enhanced, colback=green!6!white, colframe=aivnGreen, boxrule=1pt, arc=4pt,
    fonttitle=\bfseries, attach boxed title to top left={yshift=-2mm, xshift=3mm},
    colbacktitle=aivnGreen, coltitle=white, title=#2, #1
}
```

### 5.7. Hộp Tip / Mẹo — `guidelinebox`

> ⚠️ **Tên đúng là `guidelinebox`, KHÔNG phải `tipbox`.**

Nền vàng nhạt, viền cam, có icon bóng đèn.

```latex
\newtcolorbox{guidelinebox}[2][]{
    enhanced, colback=yellow!10!white, colframe=orange!90!black, boxrule=1pt, arc=4pt,
    fonttitle=\bfseries, attach boxed title to top left={yshift=-2mm, xshift=3mm},
    colbacktitle=orange!90!black, coltitle=white,
    title=\faLightbulb\hspace{1mm} #2, #1
}
```

### 5.8. Hộp Lưu ý — `notebox`

Nền vàng, viền cam, icon info, tiêu đề mặc định "Lưu ý".

```latex
\newtcolorbox{notebox}[1][]{
  enhanced, colback=yellow!10, colframe=orange!80, boxrule=1pt,
  arc=4pt, fontupper=\small, attach boxed title to top left={yshift=-2mm, xshift=3mm},
  colbacktitle=orange!80, coltitle=white, fonttitle=\bfseries,
  title=\faInfoCircle~\textbf{Lưu ý}, #1
}
```

### 5.9. Listings style

```latex
\lstdefinestyle{mystyle}{
    backgroundcolor=\color{codebackground},
    commentstyle=\color{codecomment},
    keywordstyle=\color{codekeyword},
    stringstyle=\color{codestring},
    identifierstyle=\color{codeidentifier},
    basicstyle=\ttfamily\footnotesize,
    breakatwhitespace=false,
    breaklines=true,
    captionpos=b,
    keepspaces=true,
    numbers=left,
    numberstyle=\tiny\color{codelinenumbers},
    numbersep=5pt,
    showspaces=false,
    showstringspaces=false,
    showtabs=false,
    tabsize=2
}
\lstset{style=mystyle}
```

---

## 6. Hình ảnh (3 kiểu)

### Kiểu 1: Caption hộp xanh AIO — `figurecardcaptionaivn`

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

### Kiểu 2: Caption viền xanh nền sáng — `figurecaptionmodern`

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
- **Luôn dùng `\capfig{...}` thay vì `\caption{...}`** khi ở trong `figurecardcaptionaivn` hoặc `figurecaptionmodern`

---

## 7. Bảng (4 kiểu)

### Kiểu 1: Bảng đơn giản + caption trong tcolorbox title

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

### Kiểu 2: Bảng + đoạn văn song song (2 cột)

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
Table \thetable: Evaluation results across various model configurations...
\end{captionbelowboxaivn}
\end{minipage}
```

### Kiểu 5: Bảng Ký hiệu Toán học (template chuẩn)

```latex
\begin{tcolorbox}[
    colback=white, colframe=aivnGreen, boxrule=1pt,
    title=\textbf{Bảng Ký hiệu Toán học}, fonttitle=\bfseries\color{white},
    halign title=center, colbacktitle=aivnGreen, arc=5pt
]
\renewcommand{\arraystretch}{1.5}
\rowcolors{2}{white}{green!12!white}
\begin{tabular}{>{\centering\arraybackslash}p{2.5cm} p{12cm}}
    \textbf{Ký hiệu} & \textbf{Ý nghĩa} \\
    \midrule
    $B$ & Siêu tham số, là tổng số cây quyết định trong rừng. \\
    $N$ & Tổng số điểm dữ liệu trong tập huấn luyện gốc. \\
\end{tabular}
\end{tcolorbox}
```

> **Lưu ý:** Dùng `\rowcolors{2}{white}{green!12!white}` để tô dòng chẵn/lẻ xen kẽ.

### Kiểu 6: Bảng đơn giản với đường kẻ

```latex
\renewcommand{\arraystretch}{1.3}
\setlength{\tabcolsep}{12pt}
\begin{tabular}{|c|c|}
\hline
\textbf{Điểm thi thử} & \textbf{Điểm tốt nghiệp} \\
\hline
14.0   & 12.0   \\ \hline
15.0   & 13.0   \\ \hline
\end{tabular}
\captionof{table}{Dữ liệu điểm thi thử và điểm tốt nghiệp.}
\label{tab:exam-graduation}
```

---

## 8. Code block — `aivncodebox`

### Có tiêu đề (tên file):

> ⚠️ **Bên trong `aivncodebox` dùng `\begin{python}...\end{python}`** (từ package `vipythonhighlight`), **KHÔNG phải** `\begin{lstlisting}`.

```latex
\begin{aivncodebox}[student\_management.py]
\begin{python}
# Step 1: Declare variables
student_name = "An"
math_score = 8.5
science_score = 7.0

# Step 2: Print information
print(f"Tên: {student_name} - Toán: {math_score} - Khoa học: {science_score}")
\end{python}
\end{aivncodebox}
```

### Không có tiêu đề:

```latex
\begin{aivncodebox}[]
\begin{python}
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
\end{python}
\end{aivncodebox}
```

> **Lưu ý:**
> - Ký tự `_` trong tiêu đề codebox phải escape: `student\_management.py`
> - Nếu code không phải Python, dùng `\begin{lstlisting}[language=SQL]` thay cho `\begin{python}`

---

## 9. Hộp thông tin (3 loại)

### 9.1. Hộp Khái niệm — `conceptbox`

Dùng để định nghĩa / giải thích khái niệm quan trọng.

```latex
\begin{conceptbox}{Đây là Hộp Khái niệm \faRandom}
    \textbf{Ý tưởng:} Sử dụng hộp này để định nghĩa hoặc giải thích một khái niệm quan trọng.
\end{conceptbox}
```

### 9.2. Hộp Tip / Mẹo — `guidelinebox`

> ⚠️ **Tên đúng: `guidelinebox`**, KHÔNG phải `tipbox`.

Dùng để đưa ra mẹo, hướng dẫn, quy tắc thực hành.

```latex
\begin{guidelinebox}{Đây là Hộp thông tin thêm/ mẹo/ tip \faLightbulb}
    Sử dụng hộp này để đưa ra các mẹo, hướng dẫn hoặc các quy tắc thực hành.
    \begin{itemize}
        \item Cho bài toán \textbf{phân loại}: $m \approx \sqrt{n}$.
        \item Cho bài toán \textbf{hồi quy}: $m \approx n/3$.
    \end{itemize}
\end{guidelinebox}
```

### 9.3. Hộp Lưu ý — `notebox`

Dùng để nhấn mạnh thông tin quan trọng hoặc cảnh báo.

```latex
\begin{notebox}
    Đây là hộp lưu ý, dùng để nhấn mạnh một thông tin quan trọng hoặc một cảnh báo cho người đọc.
\end{notebox}
```

### Tổng hợp nhanh:

| Mục đích | Environment | Icon |
|:---------|:------------|:-----|
| Định nghĩa / Khái niệm | `conceptbox` | `\faRandom` |
| Mẹo / Tip / Hướng dẫn | `guidelinebox` | `\faLightbulb` |
| Lưu ý / Cảnh báo | `notebox` | `\faInfoCircle` (tự có) |

---

## 10. Layout 2 cột

### Ví dụ 1: Cây quyết định + Chú thích

```latex
\begin{figure}[H]
\begin{minipage}[c]{0.45\textwidth}
    \centering
    \begin{forest}
        for tree={
            grow=south, parent anchor=south, child anchor=north,
            l sep=8mm, s sep=2mm, rounded corners, draw,
            align=center, font=\small
        }
        [``Thời tiết?'', fill=blue!20
            [``Nắng'' [``Chơi'', fill=green!40]]
            [``U ám'' [``Chơi'', fill=green!40]]
            [``Mưa'' [``Nghỉ'', fill=red!30]]
        ]
    \end{forest}
    \captionof{figure}{Cây quyết định minh họa.}
    \label{fig:dt_illustration}
\end{minipage}%
\begin{minipage}[c]{0.55\textwidth}
    \centering
    \begin{tcolorbox}[
        colback=white, colframe=blue!50, boxrule=1pt, arc=2pt,
        title=\textbf{\faInfoCircle\ Chú thích}, fonttitle=\small\bfseries,
        halign title=center, top=2pt, bottom=2pt
    ]
    \small
    \begin{itemize}[leftmargin=*, topsep=3pt, itemsep=1pt, parsep=0pt]
        \item \textbf{Nút gốc:} Nút bắt đầu (``Thời tiết?'').
        \item \textbf{Nhánh:} Kết quả phép thử (``Nắng'', ``Mưa'').
        \item \textbf{Nút lá:} Dự đoán (``Chơi'', ``Nghỉ'').
    \end{itemize}
    \end{tcolorbox}
\end{minipage}
\end{figure}
```

### Ví dụ 2: Bảng dữ liệu + Hộp phân tích

```latex
\begin{figure}[H]
\centering
\begin{minipage}{0.48\textwidth}
\centering
% Bảng dữ liệu ở đây
\captionof{table}{Dữ liệu mẫu.}
\label{tab:sample}
\end{minipage}
\hfill
\begin{minipage}{0.48\textwidth}
\RaggedRight
\begin{tcolorbox}[
  colback=gray!5!white, colframe=aivnGreen,
  fonttitle=\bfseries, title=\faInfoCircle\;Phân tích dữ liệu,
  boxrule=0.8pt, arc=3pt
]
Nội dung phân tích...
\end{tcolorbox}

\vspace{0.8em}

\begin{tcolorbox}[
  colback=orange!7!white, colframe=orange!80!black,
  fonttitle=\bfseries, title=\faTasks\;Yêu cầu,
  boxrule=0.8pt, arc=3pt
]
Nội dung yêu cầu...
\end{tcolorbox}
\end{minipage}
\end{figure}
```

---

## 11. Công thức toán

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

**Toán tử:**
```latex
\DeclareMathOperator*{\argmax}{arg\,max}
\DeclareMathOperator*{\argmin}{arg\,min}
\DeclareMathOperator{\E}{\mathbb{E}}
```

---

## 12. Câu hỏi trắc nghiệm

```latex
\begin{enumerate}[leftmargin=*]
    \item Câu hỏi ví dụ số 1?
    \begin{enumerate}[label=(\alph*)]
        \item Lựa chọn A.
        \item Lựa chọn B.
        \item Lựa chọn C.
        \item Lựa chọn D.
    \end{enumerate}

    \item Câu hỏi ví dụ số 2?
    \begin{enumerate}[label=(\alph*)]
        \item \pyth{agent.tool}
        \item \pyth{@smolagents.tool}
        \item \pyth{@tool}
        \item \pyth{@custom_tool}
    \end{enumerate}
\end{enumerate}
```

> **Lưu ý:**
> - Dùng `\pyth{...}` để inline highlight Python code trong các lựa chọn
> - Label format: `(\alph*)` → (a), (b), (c), (d)

---

## 13. Tài liệu tham khảo (Biblatex)

> ⚠️ **KHÔNG dùng** `\bibliographystyle` + `\bibliography`. Dùng `biblatex`:

```latex
\section{Tài liệu tham khảo}
\label{section:references}
\nocite{*}  % Cite tất cả entry trong .bib
\printbibliography
```

File `references.bib`:
```bibtex
@article{yao2023react,
  title={React: Synergizing reasoning and acting in language models},
  author={Yao, Shunyu and others},
  journal={arXiv preprint arXiv:2210.03629},
  year={2023}
}
```

---

## 14. Phụ lục (Appendix)

```latex
\newpage
\begin{appendices}

\section{Nội dung Phụ lục}

\begin{enumerate}
    \item \textbf{Datasets:} Các file dataset được đề cập trong bài có thể được tải tại \href{https://aivietnam.edu.vn/}{đây}.
    \item \textbf{Hint:} Các file code gợi ý có thể được tải tại \href{https://aivietnam.edu.vn/}{đây}.
    \item \textbf{Solution:} Lời giải chi tiết có thể được tải tại \href{https://aivietnam.edu.vn/}{đây}.
    \item \textbf{Rubric:}

\begin{longtable}{|c|p{7.5cm}|p{7cm}|}
    \hline
    \textbf{Mục} & \textbf{Kiến Thức} & \textbf{Đánh Giá} \\\hline
    I. &
    - Kiến thức về... \newline
    - Kiến thức về...
    &
    - Nắm được lý thuyết... \newline
    - Có khả năng triển khai...
    \\\hline
\end{longtable}
\end{enumerate}

\end{appendices}
```

---

## 15. Cấu trúc file hoàn chỉnh

```latex
% === PREAMBLE (xem mục 1) ===
\documentclass[12pt]{article}
% ... packages ...
% ... color definitions (mục 2) ...
% ... box definitions (mục 5) ...
% ... section format (mục 3) ...
% ... title/author/header (mục 4) ...

% === DOCUMENT ===
\begin{document}
\maketitle
\vspace{-3.5em}

\section{Lý thuyết}          % I.
\label{section:theory}
% ... nội dung, conceptbox, guidelinebox, notebox, hình, bảng ...

\section{Bài tập}             % II.
\label{section:practice}
% ... mô tả, code blocks ...

\newpage
\section{Câu hỏi trắc nghiệm} % III.
\label{section:multiplechoices}
% ... enumerate, choices ...

\newpage
\section{Tài liệu tham khảo}  % IV.
\label{section:references}
\nocite{*}
\printbibliography

\newpage
\begin{appendices}
\section{Nội dung Phụ lục}
% ... datasets, hints, solutions, rubric ...
\end{appendices}

\end{document}
```

---

## 16. Checklist xuất LaTeX

- [ ] Preamble có đủ packages (mục 1)?
- [ ] Tiếng Việt: `\usepackage[utf8]{vietnam}` + `babel`?
- [ ] Biblatex đã cấu hình (mục 1.1)? Dùng `\printbibliography` thay `\bibliography`?
- [ ] Màu `aivnGreen` và các màu code đã định nghĩa (mục 2)?
- [ ] Section format đúng La Mã (mục 3)?
- [ ] Logo, title, author, header/footer đúng template (mục 4)?
- [ ] Định nghĩa đủ box environments (mục 5)?
- [ ] `\noindent` trước mỗi đoạn văn?
- [ ] Hình: file tồn tại trong `Figures/`?
- [ ] Hình: caption đúng kiểu (ngắn → centering, dài → raggedright)?
- [ ] Hình: dùng `\capfig{...}` trong figurecardcaptionaivn/figurecaptionmodern?
- [ ] Bảng: `\refstepcounter{table}` cho số tự động?
- [ ] Code: trong `aivncodebox` + `\begin{python}` (KHÔNG phải `lstlisting`)?
- [ ] Hộp thông tin: đúng tên (`conceptbox` / `guidelinebox` / `notebox`)?
- [ ] Trắc nghiệm: dùng `enumerate` + `label=(\alph*)`?
- [ ] Tham khảo: `\printbibliography` (KHÔNG phải `\bibliography`)?
- [ ] Phụ lục: dùng `\begin{appendices}...\end{appendices}`?
- [ ] Công thức: `$...$` inline, `\[...\]` display?
- [ ] Ký tự đặc biệt: escape `_`, `%`, `&`, `#`, `$`, `{`, `}`?
- [ ] Encoding: UTF-8?
