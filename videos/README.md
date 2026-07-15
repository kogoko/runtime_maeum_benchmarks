# Demo Videos

**English** | 한국어는 아래 ↓

Screen recordings of the 4-panel comparison arena. Each video pairs 1:1 with a raw record —
what you see on screen is exactly what's in the JSON.

| Video file (naming) | Paired record | Language |
|---|---|---|
| `20260716_035400_lru_kr.mp4` | [`data/20260716_maeum_vs_ollama/runs/20260716_035400_2937ce.json`](../data/20260716_maeum_vs_ollama/runs/20260716_035400_2937ce.json) | Korean |
| `20260716_035900_lru_en.mp4` | [`data/20260716_maeum_vs_ollama/runs/20260716_035900_4b1eb4.json`](../data/20260716_maeum_vs_ollama/runs/20260716_035900_4b1eb4.json) | English |

**Naming convention**: `<run_id_prefix>_<topic>_<kr|en>.mp4` — the run ID prefix makes the
video ↔ record pairing verifiable.

> ⚠️ **GitHub file size limit is 100MB.** If a recording exceeds it:
> compress first (`ffmpeg -i in.mov -vcodec h264 -crf 28 out.mp4`), use Git LFS,
> or upload to YouTube/LinkedIn and put the link in the table above instead.

---

## 한국어

4패널 비교 아레나 화면 녹화. 각 영상은 원본 기록 JSON과 1:1로 짝지어진다 —
영상 속 화면 = 기록 그대로.

**파일명 규칙**: `<run_id 앞부분>_<주제>_<kr|en>.mp4` — run ID로 영상↔기록 검증 가능.

> ⚠️ **GitHub 파일 한도 100MB.** 초과 시: 압축(`ffmpeg -i in.mov -vcodec h264 -crf 28 out.mp4`),
> Git LFS 사용, 또는 YouTube/LinkedIn 업로드 후 위 표에 링크로 대체.
