---
layout: post
title: "Forget the best move. What would your opponent actually play?"
author: Nelly Mossig & Chris Eggenberger
---
*A chess engine can crush the world champion. That doesn't make its advice useful.*

## A game too big to count

The number of possible chess games, by Claude Shannon's [famous estimate](https://en.wikipedia.org/wiki/Shannon_number), is around **10^120** — more than there are atoms in the observable universe.

And yet the question of who plays it best is settled. [Stockfish](https://stockfishchess.org/), the leading open-source engine, plays at roughly 3500 [Elo](https://en.wikipedia.org/wiki/Elo_rating_system). Magnus Carlsen, the highest-rated human in history, peaked at 2882. The machines have won.

Which leaves an awkward question for the rest of us: if the engine on your phone is six hundred Elo points stronger than Carlsen, what is its advice actually worth to *you*? Its "best" move is often a quiet retreat whose point only emerges twelve plies later. Technically correct, practically useless.

> *The engine tells you what a 3500-rated player would do. It does not tell you what someone like you should try next.*

## A different question

**RealMove** is a small web tool. Enter any position, pick a rating bracket — say 1400–1500 — and it shows you the five moves that real players in that bracket actually made. Not the best move. The most *common* one. The data comes from the [Lichess Open Database](https://database.lichess.org/).

<img src="./assets/img/Real_Move_web_app.png" alt="Screenshot of the RealMove web interface showing a chessboard with the top five most-played moves listed beside it for a selected Elo bracket." style="width: 280px;">

*RealMove answers a different question: not "what's best", but "what's most likely".*

## Two billion moves per month

A single month of Lichess data is 10–20 GB compressed, containing roughly **100 million games** and **2 billion individual moves**. 

To takle this we built a three-stage pipeline: extract the raw moves, group them by position and rating, then merge into one tightly compressed [Apache Parquet](https://parquet.apache.org/) file that the live web app queries in milliseconds via [DuckDB](https://duckdb.org/).

![A clean architecture diagram showing the three pipeline stages: Stage 1 Extraction, Stage 2 Grouping, Stage 3 Merging, with the Flask web app querying the final parquet file.](./assets/img/pipeline_page.png)
*The pipeline in three stages — extract, aggregate, merge.*

## A story about bottlenecks

The conventional wisdom for too-much-data is to reach for a bigger hammer. We tried four: [pandas](https://pandas.pydata.org/) on a process pool, [Dask](https://www.dask.org/), [Polars](https://pola.rs/) (Rust-backed and routinely benchmarked at a multiple of pandas), and a [Ray](https://www.ray.io/) cluster of 256 cores across four machines.

| Approach | Hardware | Time on 20,000 games |
|---|---|---|
| Baseline (process pool + pandas) | 16 cores, one desktop | **39 s** |
| Polars (Rust-backed) | 16 cores, one desktop | 42 s |
| Dask Bag | 16 cores, one desktop | 60 s |
| Ray cluster | 256 cores, 4 nodes | **1.2 s** |

The cluster was thirty-two times faster, as expected. What was *not* expected: on the same hardware, **the most modern framework was no faster than pandas, and Dask was meaningfully slower.** The bigger hammer wasn't helping.

> *We assumed the answer was a bigger framework. We were wrong. The answer was a smaller one.*

We profiled. The bottleneck was [python-chess](https://python-chess.readthedocs.io/), the library we were using to parse the games. Its PGN reader handles every PGN file a human might write — comments, annotations, variation trees, [NAG codes](https://en.wikipedia.org/wiki/Numeric_Annotation_Glyphs). We needed none of that. We just wanted the moves.

A 60-line regex tokenizer plus `chess.Board.parse_san()` cut Stage 1 by a third:

| Parser | Hardware | Time on 20,000 games |
|---|---|---|
| `chess.pgn.read_game` (full parser) | 16 cores, one desktop | 39 s |
| Custom regex tokenizer + `chess.Board` | 16 cores, one desktop | **26 s** |

No new hardware, no new framework, no cluster — just doing less work.

<div style="page-break-after: always;"></div>

## What this means beyond chess

We learned two general Lessons:

**The bottleneck is rarely where you first look.** Our instinct was to swap out the orchestration layer. The orchestration layer was fine. The work inside it wasn't. *Profile before you scale.*

**General-purpose tools have a general-purpose cost.** python-chess is excellent software, built to handle every PGN file ever written. Our job was to handle exactly *one kind* of PGN file, as fast as possible. Sometimes the right answer is a focused sixty lines of your own code.

One honest caveat: we benchmarked on 20,000 games, so we can't yet say how the four frameworks scale to millions.

## Try it yourself

- **Code:** [github.com/BDP26/pm4-schach-analyse-bot](https://github.com/BDP26/pm4-schach-analyse-bot)

If you've ever stared at Stockfish's suggested move and thought *I would never have found that*, RealMove was built for you.

