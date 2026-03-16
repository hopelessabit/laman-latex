# LaTeX Exam Workspace

This repository is a XeLaTeX-based exam authoring workspace.
It is organized around one root entry file (`main.tex`), reusable configuration modules (`configurate/`), and many exam sets under `L10/` and `L11/`.

## 1) What this project provides

- Reusable preamble, TikZ, and pgfplots configuration.
- A dedicated exam format layer with:
  - exam/solution mode switching,
  - auto question numbering,
  - multiple-choice layouts,
  - optional QR in header,
  - optional solution/answer blocks.
- A folder-per-exam workflow (`exam_main.tex` + `part_1.tex` ... `part_4.tex`).

## 2) Requirements

- TeX distribution with `xelatex` + `latexmk` (TeX Live or MiKTeX).
- VS Code with LaTeX Workshop extension (recommended).

## 3) Quick start

### A) Compile from VS Code

1. Open this workspace folder.
2. Open `main.tex`.
3. Build with recipe: `latexmk (XeLaTeX)`.

Expected tool arguments:

- `-xelatex`
- `-synctex=1`
- `-interaction=nonstopmode`
- `-file-line-error`
- `-outdir=%OUTDIR%`
- `%DOC%`

### B) Compile from terminal (Windows PowerShell)

```powershell
latexmk -xelatex -synctex=1 -interaction=nonstopmode -file-line-error -outdir=. main.tex
```

## 4) Core architecture

`main.tex` is the main entry point. It currently loads:

- `configurate/preambles/preamble_1`
- `configurate/tikz_macro`
- `configurate/pgfplots_config`
- `configurate/formats/format_exam.tex`

Then it inputs one chosen exam file, for example:

- `L10/Kiem_tra/Giua_Ki_2/Chu_Van_An/De_tuong_tu_1/exam_main.tex`

Inside each `exam_main.tex`, you typically:

1. Select mode (`\ExamMode` or `\SolutionMode`).
2. Set exam metadata (`\ExamInfo`).
3. Optionally configure QR (`\ExamQR`, `\ShowHeaderQR`/`\HideHeaderQR`).
4. Set/render header (`\SetExamHeader`, `\MakeExamHeader`).
5. Input section files (`part_1.tex` ... `part_4.tex`).

## 5) How to create a new exam set

1. Copy an existing exam folder, for example:
   - `L10/Kiem_tra/Giua_Ki_2/Chu_Van_An/De_tuong_tu_1/`
2. Rename to your new set name (for example `De_tuong_tu_6`).
3. Update `exam_main.tex`:
   - mode,
   - exam title/code,
   - school/subject/time,
   - QR image path,
   - `\input{...}` paths for part files.
4. Edit `part_1.tex` ... `part_4.tex` content.
5. In `main.tex`, point `\input{...}` to your new `exam_main.tex`.
6. Build `main.tex`.

## 6) Writing questions in part files

Typical section header:

```tex
\ExamPart{Multiple choice}{3.0}{Answer questions 1 to 12.}
```

Question line:

```tex
\NQuestion Your question text here
```

Choices (answer key uses index 1..4):

```tex
\ChoicesOneLine{A}{B}{C}{D}{2}
\ChoicesTwoLines{A}{B}{C}{D}{4}
\ChoicesFourLines{A}{B}{C}{D}{1}
```

If you omit the 5th argument, no answer highlight is shown.

Optional solution block (shown in solution mode):

```tex
\SolutionBlock{Your explanation here.}
```

## 7) Exam mode vs solution mode

Set near the top of `exam_main.tex`:

```tex
\ExamMode
% or
\SolutionMode
```

- `\ExamMode`: no highlighted answers, no solution blocks.
- `\SolutionMode`: highlight correct choices and show solutions.

## 8) How to change configuration

### A) Build and compiler recipe (VS Code)

Update your VS Code user/workspace settings keys:

- `latex-workshop.latex.tools`
- `latex-workshop.latex.recipes`
- `latex-workshop.latex.recipe.default`

Use `latexmk` with `-xelatex` for this project because fonts and Unicode setup depend on XeLaTeX.

### B) Global preamble and spacing

Edit:

- `configurate/preambles/preamble_1.tex`

This is where you change:

- page margins (`geometry`),
- line spacing,
- fonts (`fontspec`, `unicode-math`),
- shared lengths (`\GDListItemSep`, `\GDBoxArc`, etc.).

### C) Exam layout/macros

Edit:

- `configurate/formats/format_exam.tex`

This is where you change:

- exam header and footer behavior,
- labels (`\SetSolutionTitle`, `\SetAnswerLabel`, etc.),
- question/part counters,
- choice layouts and answer highlighting,
- dotted answer lines.

### D) Plot style defaults

Edit:

- `configurate/pgfplots_config.tex`

This is where you change:

- global axis defaults,
- grid presets (`grid on`, `grid off`),
- axis styles (`base`, `cartesian`, `exam`),
- plot styles (`function`, `discontinuity`, `points`).

## 9) Recommended workflow

1. Keep all macro definitions in `configurate/` files.
2. Keep `exam_main.tex` focused on metadata + part includes.
3. Keep all question content in `part_*.tex`.
4. Compile only through `main.tex`.
5. Use relative paths consistently with `\input{...}` and `\ExamQR{...}`.

## 10) Troubleshooting

### Build fails with missing file

- Check exact relative path in `\input{...}`.
- Keep folder name casing consistent (`Giua_Ki_2` is used in the repository).

### Font errors on first compile

- Ensure XeLaTeX is used, not pdfLaTeX.
- Ensure required fonts from preamble are available.

### Header QR not displayed

- Ensure `\ShowHeaderQR` is enabled.
- Ensure image path in `\ExamQR{...}` is correct.

### Answers are not highlighted

- Ensure `\SolutionMode` is enabled.
- Ensure choice macros include the 5th numeric answer argument.

## 11) Reference docs in this repo

- `documents/how_to_use_format_exam.md`
- `documents/how_to_use_format_2.md`
- `documents/complete_guide_format_2.md`
- `documents/pgfplots_config_usage.md`
- `documents/pgfplot_config_guide.md`
- `documents/fixing_guide_issue_drawSplitEllipse_undefined_coords.md`

## 12) Minimal template

```tex
% in main.tex
\documentclass[12pt, a4paper]{book}

\input{configurate/preambles/preamble_1}
\input{configurate/tikz_macro}
\input{configurate/pgfplots_config}
\input{configurate/formats/format_exam.tex}

\begin{document}
\input{L10/Kiem_tra/Giua_Ki_2/Chu_Van_An/De_tuong_tu_1/exam_main.tex}
\end{document}
```

---

If you want, this README can be split next into:

- `README.en.md` (English),
- `README.vi.md` (Vietnamese),

while keeping `README.md` as a short index.
