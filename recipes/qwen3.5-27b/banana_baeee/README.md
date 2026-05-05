# qwen3.5-27b-fp8-dflash-vllm-banana_baeee

Qwen3.5-27B-FP8 + DFlash speculative decoding (`z-lab/Qwen3.5-27B-DFlash`, `num_speculative_tokens=15`) on a single DGX Spark GB10 (122 GiB unified memory).

## Run

```bash
sparkrun arena benchmark @community/qwen3.5-27b-fp8-dflash-vllm-banana_baeee
```

Server startup ~16 min. Full 28-cell `spark-arena-v1` ladder (n=3) ~12.8 h.

## Results

`tg128` aggregate tok/s (sum across concurrent requests), n=3, full `spark-arena-v1` ladder. Single DGX Spark GB10.

| depth | c=1 | c=2 | c=5 | c=10 |
|---:|---:|---:|---:|---:|
| 0 | 24.5 ± 2.4 | 28.2 ± 6.2 | 62.1 ± 7.4 | **105.0 ± 6.9** |
| 4 096 | 21.6 ± 0.3 | 36.0 ± 2.7 | 56.1 ± 13.2 | 12.1 ± 1.8 |
| 8 192 | 20.2 ± 7.6 | 28.7 ± 6.8 | 29.4 ± 4.3 | 5.2 ± 0.7 |
| 16 384 | 18.1 ± 1.4 | 31.7 ± 2.9 | 27.4 ± 14.5 | 3.8 ± 0.3 |
| 32 768 | 17.3 ± 1.7 | 26.0 ± 1.2 | 21.2 ± 10.3 | 2.0 ± 0.1 |
| 65 535 | 9.8 ± 1.3 | 16.0 ± 0.9 | 4.4 ± 2.1 | 1.4 ± 0.1 |
| 100 000 | 7.5 ± 0.2 | 11.8 ± 0.6 | 1.0 ± 0.0 | 0.8 ± 0.0 |

n=3 has high variance — an independent n=30 measurement at d=0 c=1 gave 24.78 ± 4.10, p10=18.81, p90=30.78.

## Pitfalls

- Read `benchmarks[*].tg_throughput.mean` from `result.json` — not `peak_throughput`.
- Depth labels are GPT-2 tokens (llama-benchy fallback when `served-model-name` isn't an HF id). Output throughput is in true Qwen tokens.

## Author

[@banana_baeee](https://twitter.com/banana_baeee)
