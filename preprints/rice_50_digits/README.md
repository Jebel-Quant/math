# The Rice SIAM chapter 50-digit challenge

Thomas Schmelzer. Written in 2005; this is a corrected edition.

In 2005 the Rice University SIAM student chapter set five new 10-digit
problems, modelled on Trefethen's SIAM 100-digit challenge (Bornemann et al.).
This note gives a solution to each problem, proposes a sixth, and has an
appendix on the integral

    ∫₀¹ sin²(tan(tan(πx))) dx ≈ 0.3909921622

## Problems and answers

| # | Problem                                                                  | Method                                     | Answer          |
|---|--------------------------------------------------------------------------|--------------------------------------------|-----------------|
| 1 | Two knights jump at random on a chessboard. What is P(a knight is on a corner after 2005 turns)? | Markov chain on pairs of squares | 0.04849375459 |
| 2 | A photon is trapped between two elliptic mirrors. How far is it from the origin at t = 60? | Exact ray tracing, one quadratic per bounce | 2.862335868 |
| 3 | How large is the root nearest the origin of Σ pₖ₊₁ xᵏ, k ≤ 10 000?       | Truncation, companion matrix, Rouché       | 0.8065135993    |
| 4 | Five point masses under gravity. How far is p₄ from the origin at t = 5? | Taylor-series integration in high precision | 1.894509796    |
| 5 | Two balls loose in an open spherical "fishbowl". How far apart are their centres at t = 10? | Planar reduction, elastic collisions | 0.5433103836 |

## What changed since 2005

- **Section 6 (Problem 5) was wrong.** On impact the balls swapped their whole
  velocity vectors, which is only right for a head-on collision. The answer
  is 0.5433103836; the 2005 submission was 1.016560194.
- **Section 3 (Problem 2) drew the wrong conclusion.** The "loss of 43 digits"
  was the worst-case bound tracked by significance arithmetic. The billiard
  itself costs about 1.5 digits over the whole flight.
- **Section 5 (Problem 4) checked its answer the wrong way.** Perturbing the
  initial data measures conditioning, not integration error. The two are now
  measured separately.
- **Section 4 (Problem 3) had a gap.** The truncation bound controls function
  values but not where the roots are. Rouché's theorem closes it.

The original MATLAB and Mathematica code has been rewritten in Python.

## Reproducibility

Every number, table, figure and code listing in the paper is produced by
`code/paper.py`. None of them are typed into the LaTeX by hand. Listings come
from regions of the source marked with `# <<name … # >>name` sentinels, so the
code you read in the PDF is the code that ran.

```
source/
├── digit50.tex      the paper (plain article: amsmath, amssymb, graphicx)
├── Makefile
├── code/            one module per problem, plus taylor.py, integral.py,
│                    snippets.py and the driver paper.py
├── figs/            generated figures
├── tables/          generated tables and numeric macros (numbers.tex)
└── snippets/        generated code listings
```

## Building

You need [uv](https://astral.sh/uv) and a TeX installation with `pdflatex`.
Run these from `source/`:

| Command        | What it does                                                         |
|----------------|----------------------------------------------------------------------|
| `make assets`  | Run `code/paper.py` to regenerate `figs/`, `tables/` and `snippets/` |
| `make compile` | Build `digit50.pdf`, rerunning `make assets` first if any `code/*.py` changed |
| `make check`   | List inputs that `digit50.tex` references but that are missing       |
| `make arxiv`   | Pack the LaTeX source and generated inputs into `digit50-arxiv.zip`  |
| `make view`    | Open the PDF                                                         |
| `make clean`   | Delete LaTeX intermediates                                           |

The Python dependencies (mpmath, numpy, scipy, sympy, matplotlib) are listed
inline in `code/paper.py`, and `uv run` installs them automatically.
