# qwen3.5-27b-dflash-fp8-vllm

Qwen3.5-27B-FP8 + DFlash speculative decoding (`z-lab/Qwen3.5-27B-DFlash`, `num_speculative_tokens=15`) on a single DGX Spark.

## Run

```bash
sparkrun arena benchmark @community/qwen3.5-27b-dflash-fp8-vllm
```

Server startup ~16 min. Full ladder ~7 hours.

## Results

`tg128` aggregate tok/s (sum across concurrent requests). Single DGX Spark GB10, 122 GiB unified memory. Default `llama-benchy 0.3.7`.

| depth | c=1 | c=2 | c=5 | c=10 |
|---:|---:|---:|---:|---:|
| 0 | 26.29 | 39.43 | 71.90 | **101.75** |
| 4096 | 22.31 | 38.37 | 65.71 | 70.63 |
| 8192 | 23.42 | 39.10 | 51.29 | 18.30 |
| 16384 | 24.38 | 27.68 | 11.77 | 7.46 |
| 32768 | 16.50 | 20.61 | 4.27 | 3.11 |
| 65535 | 13.08 | 5.64 | 1.88 | 1.55 |
| 100000 | 8.11 | 2.24 | 1.09 | _pending_ |

Per-cell JSON in `results/cell-d{depth}-c{concurrency}.json`.

## Pitfalls

- `llama-benchy ≤ 0.3.5` undercounts spec-decode tok/s ~5×. Use `0.3.6+`.
- Read `tg_throughput.mean` from result JSONs — not `peak_throughput`.
- Depth labels are GPT-2 tokens (llama-benchy fallback when served-model-name isn't an HF id). Output throughput is in true Qwen tokens.

## Author

[@banana_baeee](https://twitter.com/banana_baeee)
