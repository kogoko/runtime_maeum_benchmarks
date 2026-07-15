# Demo Videos

**English** | 한국어는 아래 ↓

Screen recordings (QuickTime `.mov`) of the 4-panel comparison arena. Each video pairs 1:1
with a raw record — what you see on screen is exactly what's in the JSON.

| Video file | Paired record | Language |
|---|---|---|
| `20260716_035400_lru_kr.MOV` | [`data/20260716_maeum_vs_ollama/runs/20260716_035400_2937ce.json`](../data/20260716_maeum_vs_ollama/runs/20260716_035400_2937ce.json) | Korean |
| `20260716_035900_lru_en.MOV` | [`data/20260716_maeum_vs_ollama/runs/20260716_035900_4b1eb4.json`](../data/20260716_maeum_vs_ollama/runs/20260716_035900_4b1eb4.json) | English |

**Naming convention**: `<run_id_prefix>_<topic>_<kr|en>.MOV` — the run ID prefix makes the
video ↔ record pairing verifiable. GitHub's file viewer plays `.mov` inline.

> ⚠️ **GitHub file size limit is 100MB** — QuickTime screen recordings get large fast.
> If a `.mov` exceeds it, compress to H.264 (usually 5–10× smaller, quality fine for screen text):
>
> ```bash
> ffmpeg -i input.mov -vcodec h264 -crf 28 -preset slower -acodec aac output_compressed.mov
> # 또는 mp4 컨테이너로: ffmpeg -i input.mov -vcodec h264 -crf 28 output.mp4
> ```
>
> Still too big → Git LFS, or upload to YouTube/LinkedIn and replace the filename in the
> table above with the link.

---

## 한국어

4패널 비교 아레나 화면 녹화 (QuickTime `.mov`). 각 영상은 원본 기록 JSON과 1:1로
짝지어진다 — 영상 속 화면 = 기록 그대로.

**파일명 규칙**: `<run_id 앞부분>_<주제>_<kr|en>.MOV` — run ID로 영상↔기록 검증 가능.
GitHub 파일 뷰어에서 `.mov` 인라인 재생 지원.

> ⚠️ **GitHub 파일 한도 100MB** — 퀵타임 화면 녹화는 금방 커진다. 초과 시 H.264 압축
> (보통 5–10배 축소, 화면 텍스트 품질 무손실 체감):
>
> ```bash
> ffmpeg -i 원본.mov -vcodec h264 -crf 28 -preset slower -acodec aac 압축본.mov
> ```
>
> 그래도 크면 → Git LFS, 또는 YouTube/LinkedIn 업로드 후 위 표의 파일명을 링크로 대체.
