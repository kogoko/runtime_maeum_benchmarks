# MAEUM Runtime — Benchmarks

**English** | [한국어](README.ko.md)

> **"Quality doesn't come from model size. It comes from the runtime."**

Public benchmark records for **MAEUM Runtime** — an on-device sLLM runtime for Apple Silicon (MLX)
that wraps small models in a full pipeline (dynamic prompt routing, session memory, KV prefix cache,
reasoning modes, grounding enforcement) so they answer like frontier AI. 100% local; data never
leaves the machine.

This repo publishes **every raw record** behind our claims: full transcripts, per-panel metrics
(TTFT / tok/s / total time), and honest analysis — including the metrics we lose.

---

## 🏆 Headline Result (2026-07-16)

Same machine (M5 MacBook) · same model families · **same 4-bit quantization** · same queries.
MAEUM Runtime vs raw Ollama, 4 panels streaming simultaneously. 14 runs, all published.

**Aggregate (median, n=13 — one cache-hit run excluded for fairness):**

| Panel | TTFT | tok/s | Output tokens | Completion |
|---|---|---|---|---|
| **MAEUM · Gemma** (E2B, 4-bit) | 11.2s | 16.7 | **1,509** | **14/14** |
| raw Ollama · Gemma (same 4-bit class) | 27.9s | 5.4 | 256 | suspected truncation 9/13 |
| MAEUM · Qwen (4B, 4-bit) | 13.9s | 12.2 | 884 | 14/14 |
| raw Ollama · Qwen (same 4-bit class) | **3.6s** | 16.7 | 912 | 13/13 |

**Behavioral comparison (identical queries — full transcripts in [`data/.../runs/`](data/20260716_maeum_vs_ollama/runs/)):**

| Test | MAEUM | raw |
|---|---|---|
| "What time is it right now?" | ✅ Exact date & time | ❌ **Asserted a fake date** |
| Nonexistent book by a real author | ✅ Refused: "cannot be verified" | ❌ **Invented a fake author name** |
| Fake API (`json.super_parse`) | ✅ Refused | ✅ Refused |

**Honest scoreboard**: raw Qwen wins time-to-first-token (3.6s vs 13.9s) — the cost of full-pipeline
context assembly, and our #1 optimization target. We publish the metrics we lose, so you can trust
the ones we win.

→ **[Full report](data/20260716_maeum_vs_ollama/README.md)** · [Per-run detail table](data/20260716_maeum_vs_ollama/runs_detail.md)

---

## Methodology

- **Arena**: MAEUM's built-in 4-panel comparison page — one query fires all four panels
  simultaneously; metrics measured client-side at the identical point for all panels.
- **Fairness controls**: same 4-bit quantization class per family
  (`qwen3_4b` MLX ↔ `qwen3:4b-q4_K_M` / `gemma-4-e2b-it` OptiQ MLX ↔ `gemma4:e2b-it-q4_K_M`),
  web search disabled, one cache-hit run excluded from aggregates and labeled in the data.
- **Battery**: 10 behavioral queries (time awareness · reasoning traps · hallucination defense ·
  Korean structuring · repetition · role-invasion) + repeated coding task (KR/EN).
- **Records**: every run auto-saved as JSON — prompt, all four full outputs, reasoning streams,
  TTFT, token counts, tok/s, model tags, status.

**Known limitations** (also in the report): single execution per query (LLM outputs are stochastic;
the coding task was repeated 4× with the same pattern) · root cause of raw Gemma degradation not
diagnosed (may be serving defaults/template) · truncation detection is heuristic.

---

## About the Runtime

The runtime itself is currently in **private beta** (patent-pending components).

- 🎬 Demo videos: [`videos/`](videos/) (paired 1:1 with run records) · pinned posts on [LinkedIn — Lee, DongHun](https://www.linkedin.com/in/donghunlee-princeps)
- 🤝 Demo / licensing / partnership inquiries: **[LinkedIn](https://www.linkedin.com/in/donghunlee-princeps) or GitHub issues**
- 🧭 Positioning: not a model loader — a runtime. Web RAG + session/personal memory +
  token-budget context management + auto reasoning modes + white-label identity,
  exposed through OpenAI- and Claude-compatible APIs.

*Developer: Lee, DongHun — L co.*

---

## License

Benchmark data, transcripts, and reports in this repository are released under
**CC BY 4.0** — free to share and cite with attribution.
