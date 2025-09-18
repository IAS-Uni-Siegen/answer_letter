# Response-to-Reviewers Letter Template

This repository provides a LaTeX template to write clear, well-structured responses to editors and reviewers during scientific peer review.

The main files are:
- `reviewresponse.sty` — the style file with all environments and commands
- `main.tex` — an example letter showing typical usage


## Quick start

1) Set your metadata near the top of `main.tex`:
- `\paperid{...}`
- `\papertitle[Short Title]{Long Title}`

2) For each editor/reviewer, start a section and add Q/A items:
- `\reviewersection{Reviewer 1}`
- `\begin{question}[q:label] ... \end{question}`
- `\begin{answer}[a:label] ... \end{answer}`

3) Reference items anywhere in the letter:
- Questions: `\qref{q:label}` → “Question 1.2”
- Answers: `\aref{a:label}` → “Answer 1.2”
- Figures: `\figref{fig:label}` → “Fig. R1”
- Tables: `\tableref{tab:label}` → “Tab. R1”

4) Compile (uses biblatex + biber):

```powershell
# Minimal manual sequence
pdflatex main.tex
biber main
pdflatex main.tex
pdflatex main.tex

# Or with latexmk (if installed)
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```


## Core concepts and commands

Metadata
- `\paperid{...}` set once; `\printpaperid` prints it
- `\papertitle[Short]{Long}` sets both long and optional short titles; `\printpapertitle` prints the long title

Reviewer sections and numbering
- `\reviewersection{<name>}` starts a reviewer/editor block and resets the per-reviewer counter
- Questions/Answers are numbered as `<reviewer>.<item>` (e.g., 2.3)

Q/A environments (boxed)
- `\begin{question}[<opt-label>] ... \end{question}`
- `\begin{answer}[<opt-label>] ... \end{answer}`
- Optional labels let you cross-reference specific items
- Boxes have no first-line indent; a small vertical space follows each answer to visually group Q/A pairs

Cross-referencing helpers
- `\qref{q:...}` → “Question <rev.item>”
- `\aref{a:...}` → “Answer <rev.item>”
- `\figref{fig:...}` → “Fig. R<n>” (uses your configured `\figurename`)
- `\tabref{tab:...}` or `\tableref{tab:...}` → “Tab. R<n>` (uses your configured `\tablename`)

Figures and tables inside answers (no floats)
- Floats (\`figure\`, \`table\`) don’t play well inside framed environments
- Instead, embed content with `\captionof` from the `caption` package:

```latex
\begin{center}
	\begin{minipage}{0.9\linewidth}
		% your graphic/table here
		\captionof{figure}{A non-floating figure inside an answer.}
		\label{fig:R-example}
	\end{minipage}
\end{center}
```

Numbering conventions (R-prefix)
- Equations: `R<num>` (e.g., R3)
- Figures: `R<num>` shown via `\figref{...}` as “Fig. R<num>”
- Tables: `R<num>` shown via `\tableref{...}` as “Tab. R<num>”

Bibliography
- Uses `biblatex` (style: ieee) and `biber`
- Add entries to `references.bib` and end your document with `\printbibliography`
- Citation links are black for easy printing: `\usepackage[colorlinks=true,linkcolor=blue,citecolor=black]{hyperref}`


## Minimal example

```latex
\paperid{ABC-2025-0001}
\papertitle[Short Response Title]{Full Response Title Used on the Front Page}

\reviewersection{Reviewer 1}

\begin{question}[q:scope]
Please clarify the scope of your added experiments.
\end{question}

\begin{answer}[a:scope]
We expanded Section~IV and added an ablation (see \tableref{tab:ablation}).
\begin{center}
	\begin{minipage}{0.9\linewidth}
		\centering
		\begin{tabular}{lc} \hline
			Variant & Accuracy \\ \hline
			Baseline & 0.90 \\
			+ Augment & 0.93 \\
			+ Filter & 0.92 \\ \hline
		\end{tabular}
		\captionof{table}{A small ablation table.}
		\label{tab:ablation}
	\end{minipage}
\end{center}
\end{answer}

As noted in \qref{q:scope}, we now provide reproducible settings.
```


## Tips and troubleshooting

- Avoid `\begin{figure}` / `\begin{table}` floats inside `question`/`answer` boxes; use `\captionof` instead
- To reference equations, use standard `\eqref{...}`; they’ll appear as R-prefixed (e.g., R5)
- If a reference shows as “??”, re-run the LaTeX cycle until references resolve (pdflatex → biber → pdflatex ×2)
- The footer page number is centered across the physical page; no header is used by default


## Customization

- Change names: `\renewcommand*{\figurename}{Fig.}` / `\renewcommand*{\tablename}{Tab.}`
- Change numbering: update the `\theequation` / `\thefigure` / `\thetable` redefinitions in `reviewresponse.sty`
- Spacing: adjust the `\vspace{...}` at the end of the `answer` environment if you want tighter/looser grouping


## License

See `LICENSE` for details.