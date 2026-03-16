# Full Guide: How To Use format_exam.tex

This guide explains the complete workflow for using your exam format macros.

You already have main.tex set up, so this document focuses on how to write exam_main.tex and part files correctly.

## 1) Current Architecture

Your current compile chain is:

1. main.tex loads preamble and format files
2. main.tex inputs one exam_main.tex
3. exam_main.tex sets mode, exam info, header, then inputs part_1.tex, part_2.tex, part_3.tex, part_4.tex

Important: Do not redefine the exam macros inside part files. Keep all macro definitions in configurate/formats/format_exam.tex.

## 2) Mode Control: Exam vs Solution

Two global modes exist:

    \ExamMode
    \SolutionMode

Behavior:

- Exam mode:
  - Choices are shown
  - No answer highlight
  - No SolutionBlock output
  - No AnswerLine output

- Solution mode:
  - Correct choices are highlighted
  - SolutionBlock is shown
  - AnswerLine is shown

Recommended placement in exam_main.tex:

    \SolutionMode

or

    \ExamMode

Put this near the top of exam_main.tex before any question content is input.

## 3) Header Info

You can set all header fields with:

    \ExamInfo
      {School name}
      {Exam title}
      {Subject}
      {Duration}
      {Note}
      {Exam code}

Then render header:

    \SetExamHeader{Header right text}{Page label}
    \MakeExamHeader

Optional student line:

    \MakeStudentInfoLine

## 4) QR In Header (Column 3)

QR is controlled separately from exam/solution mode.

Set QR image path:

    \ExamQR{relative/path/to/qr.png}

Show QR:

    \ShowHeaderQR

Hide QR:

    \HideHeaderQR

Notes:

- Default is hidden.
- Use image path directly. Do not wrap image path with input.
- Correct:

    \ExamQR{L10/Kiem_tra/Giua_Ki_2/Nam_Ha/De_tuong_tu_2/images/qrcode1.png}

- Incorrect:

    \ExamQR{\input{...png}}

## 5) Section and Question Structure

Create a section:

    \ExamPart{Section title}{Points}{Instruction text}

Create a question line:

    \NQuestion Question text here

(You can also use Question.)

## 6) Multiple Choice: Correct Syntax (1-Based Answer Index)

Your current macros use numeric answer keys:

- 1 means option A
- 2 means option B
- 3 means option C
- 4 means option D

### 6.1 One line layout

    \ChoicesOneLine{Choice A text}{Choice B text}{Choice C text}{Choice D text}{2}

This highlights B in SolutionMode.

### 6.2 Two lines layout

    \ChoicesTwoLines{Choice A text}{Choice B text}{Choice C text}{Choice D text}{4}

This highlights D in SolutionMode.

### 6.3 Four lines layout

    \ChoicesFourLines{Choice A text}{Choice B text}{Choice C text}{Choice D text}{1}

This highlights A in SolutionMode.

### 6.4 Without answer key

All three choice macros also support 4 arguments only:

    \ChoicesOneLine{A}{B}{C}{D}

In this case no highlight is applied.

## 7) Very Important: Do Not Use Letter Keys Now

Old style answer key like A/B/C/D is no longer the active standard.

Use numeric key 1 to 4.

Examples:

- Old style: {B}
- New style: {2}

## 8) AnswerLine and SolutionBlock

### 8.1 SolutionBlock

    \SolutionBlock{Detailed explanation here.}

Shown only in SolutionMode.

### 8.2 AnswerLine

    \AnswerLine{Final answer text}

Shown only in SolutionMode.

Important for multiple choice:

- If you add AnswerLine after choices, it creates a separate line below the options.
- If you only want inline highlight on the correct option, do not add AnswerLine for that question.

## 9) True/False Questions

Use SubQuestions with TFItem:

    \NQuestion Xét các mệnh đề sau:
    \begin{SubQuestions}
      \TFItem{true}{Mệnh đề thứ nhất.}
      \TFItem{false}{Mệnh đề thứ hai.}
    \end{SubQuestions}

TFItem is case-insensitive for true and false.

## 10) Labels You Can Customize

You can customize texts used in solution output:

    \SetSolutionTitle{Hướng dẫn giải}
    \SetAnswerLabel{Đáp số:}
    \SetTrueSymbol{Đ}
    \SetFalseSymbol{S}

Set these in exam_main.tex before content if you want custom wording.

## 11) Practical exam_main.tex Template

    \SolutionMode

    \ExamInfo
      {SỞ GD \& ĐT ...}
      {ĐỀ KIỂM TRA ...}
      {TOÁN 10}
      {90 phút}
      {không kể thời gian phát đề}
      {N02}

    \ExamQR{L10/Kiem_tra/Giua_Ki_2/Nam_Ha/De_tuong_tu_2/images/qrcode1.png}
    \ShowHeaderQR

    \SetExamHeader{Đề kiểm tra Toán 10}{Trang}
    \MakeExamHeader
    \MakeStudentInfoLine

    \input{L10/Kiem_tra/Giua_Ki_2/Nam_Ha/De_tuong_tu_2/part_1.tex}
    \input{L10/Kiem_tra/Giua_Ki_2/Nam_Ha/De_tuong_tu_2/part_2.tex}
    \input{L10/Kiem_tra/Giua_Ki_2/Nam_Ha/De_tuong_tu_2/part_3.tex}
    \input{L10/Kiem_tra/Giua_Ki_2/Nam_Ha/De_tuong_tu_2/part_4.tex}

## 12) Practical part_1.tex Template (MC + explanation)

    \ExamPart{Trắc nghiệm nhiều phương án lựa chọn}{3.0}{Thí sinh trả lời từ câu 1 đến câu 12. Mỗi câu thí sinh chỉ chọn một phương án.}

    \NQuestion Tam thức bậc hai f\left(x\right)=x^2-4x+3 âm khi
    \ChoicesOneLine
    {$x\in(-\infty;1)\cup(3;+\infty)$.}
    {$x\in(1;3)$.}
    {$x\in[1;3]$.}
    {$x\in(-\infty;1]\cup[3;+\infty)$.}
    {2}
    \SolutionBlock{Ta có ...}

## 13) Build Command

From workspace root:

    latexmk -g -xelatex -interaction=nonstopmode -halt-on-error main.tex

## 14) Troubleshooting Checklist

If highlight does not appear:

1. Confirm SolutionMode is active in exam_main.tex
2. Confirm the 5th argument is numeric 1 to 4
3. Confirm you are compiling the correct main.tex
4. Force rebuild with latexmk -g
5. Ensure choice macros are called as 4 choices first, answer index last

If QR does not appear:

1. Confirm ShowHeaderQR is called
2. Confirm ExamQR path is valid
3. Use direct image path, not input
4. Avoid special filename characters if path issues occur

If QR vertical alignment looks off:

- Current header macro already uses top-aligned minipages with vspace 0pt and centering for better alignment.

## 15) Recommended Style Conventions

1. Keep all macro definitions inside configurate/formats/format_exam.tex only.
2. Keep exam_main.tex for global settings and file inputs.
3. Keep question content only inside part files.
4. Use numeric answer keys consistently in all multiple-choice parts.

---

If you want, the next step can be a migration pass for other exam folders to convert all old letter answer keys to 1-based numeric keys automatically.
