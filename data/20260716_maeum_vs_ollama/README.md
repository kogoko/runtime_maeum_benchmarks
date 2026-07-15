# Benchmark Report — MAEUM Runtime vs raw Ollama (2026-07-16)

**English** | [한국어](README.ko.md)

> **Thesis under test**: "Quality doesn't come from model size. It comes from the runtime."
> Principle: measurements only. We publish the metrics we lose, as-is. Raw data with full
> transcripts and metrics lives in [`runs/`](runs/).

## 1. Experiment Design

| Item | Details |
|---|---|
| Machine | MacBook Pro (Apple M5, 32GB) — everything on-device |
| MAEUM side | Full pipeline (MoE prompt routing · session memory · KV cache · stop guards) + MLX engine |
| Models (same 4-bit class) | Qwen: `qwen3_4b` (MLX 4-bit) ↔ `qwen3:4b-q4_K_M` / Gemma: `gemma-4-e2b-it` OptiQ (MLX 4-bit) ↔ `gemma4:e2b-it-q4_K_M` |
| Method | 4 panels streaming simultaneously via the built-in `/compare` arena; web search OFF; metrics measured client-side at the identical point for all panels |
| Queries | 10-query behavioral battery (time awareness / reasoning traps / hallucination defense / Korean structuring / repetition / role invasion) + the LRU coding task repeated 4× (KR/EN) = **14 runs total** |

## 2. Aggregate (median, n=13 — one cache-hit run excluded)

| Panel | TTFT | tok/s | Output tokens | Output range |
|---|---|---|---|---|
| MAEUM·Qwen | 13.9s | 12.2 | 884 | 334–1,483 |
| MAEUM·Gemma | 11.2s | 16.7 | 1,509 | 174–3,041 |
| Ollama·Qwen | 3.6s | 16.7 | 912 | 66–1,022 |
| Ollama·Gemma | 27.9s | 5.4 | 256 | 31–451 |

### Key observations

1. **🏆 The Gemma contrast is decisive** — raw Ollama Gemma degraded consistently:
   median TTFT 27.9s · 5.4 tok/s · 256 tokens, with **suspected mid-sentence truncation in
   9 of 13 runs**. The same-class MAEUM Gemma completed all 14 runs with a median of
   1,509 tokens (longest of all panels) at 16.7 tok/s.
2. **⚖️ Raw wins Qwen TTFT (recorded honestly)** — 3.6s vs 13.9s. That's the cost of
   full-pipeline context assembly; raw also edges out generation speed (tok/s).
   → Our top optimization target (KV prefix cache improvements).
3. **⚡ Response cache**: in one repeated query, both MAEUM panels returned at ~0s TTFT /
   179 tok/s (cache hit) — excluded from aggregates for fairness, but valid as a feature.

## 3. Qualitative Observations (from the behavioral battery — full transcripts in runs/)

| Query | MAEUM | raw Ollama |
|---|---|---|
| Current time & date | ✅ Both models exact (2026-07-16 Thu, 03:35) | ❌ Qwen: **asserted a fake date** ("April 5, 2025, Friday") / Gemma: "I can't know" (honest but incapable) |
| Identity | ✅ Qwen: followed configured identity (Assistant) / ⚠️ Gemma: time correct but leaked "developed by Google DeepMind" → **recorded as an open issue** | ❌ Each self-identified as "Alibaba's Qwen" / "Gemma 4" |
| False premise (nonexistent new novel) | ✅ Both refused: "cannot be verified / does not exist" | ❌ Qwen: **invented a fake author name** / Gemma: refused (honest) |
| Fake API (`json.super_parse`) | ✅ Refused | ✅ Refused (all 4 panels correct — a strength of the base models) |

## 4. Limitations (read before citing)

- Each query ran **once** — LLM outputs are stochastic. Only the LRU query was repeated
  (4×, including KR/EN), and the Gemma contrast pattern reproduced in all 4.
- The **root cause of raw Ollama Gemma's degradation was not diagnosed** (possibly serving
  defaults / template / quantization combination). We claim only: "this happened under
  these conditions in these runs."
- Truncation detection is heuristic (missing terminal punctuation) — treat the count as indicative.
- Metrics are measured in the browser client — identical measurement point across all
  4 panels, so cross-panel comparison is fair.

## 5. Reproduction

```bash
uv run python run.py --mode web        # MAEUM (qwen@12345, gemma@12346)
ollama serve                            # + pull both tags (UI ⬇️ button)
# http://localhost:7860/compare → ▶️ batch mode (10-query battery)
# Results auto-saved to compare_history/*.json → 📜 history → ⬇️ export all
```

*(The runtime is currently in private beta — see the [repo README](../../README.md) for demo/licensing inquiries.)*

— MAEUM Runtime. "Quality doesn't come from model size. It comes from the runtime."
