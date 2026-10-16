# Recorded results and measurement scope

These results summarize preserved internal reports from 12-13 October 2026 for NK8's experimental L8 profile. The public documentation was prepared on 16 October 2026. No benchmark was rerun for this publication.

The full source corpus, NK8 archives, raw internal reports and implementation code are not distributed here. Three attributed visual comparisons are provided in [Examples](EXAMPLES.md). These are developer-reported measurements, not a publicly reproducible benchmark release or an independent codec comparison.

## Larger corpus: size and fidelity

| Property | Value |
|---|---:|
| Distinct images | 2,506 |
| Source bytes | 9,848,090,481 |
| Source pixels | 6,903,142,283 |
| Tiles | 5,165 |
| Complete archive bytes | 1,392,634,048 |
| Reconstructed RGB bytes | 20,709,426,849 |
| Source-file/archive ratio | 7.0716:1 |
| Archive/source-file fraction | 14.1412% |
| Source-file size reduction | 85.8588% |
| Mean RGB8 PSNR | 42.024456 dB |

The denominator for the size reduction is the source files' encoded byte size, not their uncompressed RGB footprint. The result depends on the source formats, their existing compression, image content and image dimensions. It is not a universal archive-reduction promise.

PSNR was measured against normalized source pixels without resizing, separately from decode timing. A mean PSNR does not describe every image or guarantee subjective acceptance. The larger corpus has no alpha channel; separate dedicated checks cover alpha and metadata behavior.

## Larger corpus: warm decoding

The initial larger-corpus campaign recorded these three passes:

| Pass | Seconds |
|---|---:|
| 1 | 18.812879 |
| 2 | 18.856922 |
| 3 | 18.969954 |

Median: **18.856922 s**, approximately **366.1 MP/s**.

A later process-optimization campaign retained a decode profile with wider prepared-cache coverage. It recorded **16.483 s** on the same 9.848 GB corpus, compared with **18.755 s** for its control, approximately **12.1% less time**. This is a separate campaign, not the next sample in the three-pass series above. Its derived throughput is approximately **418.8 MP/s**.

The later campaign reports preserved RGB and metadata equality against its independent reference before and after timing. It does not change the encoder, archive format or model.

## Smaller separately prepared panel

| Property | Value |
|---|---:|
| Images | 257 |
| Source bytes | 1,005,280,008 |
| Pixels | 714,877,000 |
| Tiles | 529 |
| Complete archive bytes | 139,875,968 |
| Retained warm-decode reference | 1.721952 s |
| Approximate derived throughput | 415.2 MP/s |

The associated five-pass comparison reports 1.7213 s for its preceding decoder and 1.7220 s for the retained profile. It finds **no measurable additional gain on this panel**. Preparation was restricted to this panel, unlike earlier measurements prepared on the larger corpus. A sub-one-second result is not claimed.

## What the warm-decode timer includes

- Reading local archives with a warm file cache.
- Archive integrity and entropy index/symbol checks.
- Native CPU entropy decoding.
- GPU neural reconstruction.
- RGB8 assembly in host memory and streaming consumption.

The timer excludes model loading, graph/table preparation and qualification, final RGB hash comparisons, quality calculations, and writing PNG/JPEG files. It does not measure cloud retrieval, first installation, cold storage access or complete application latency.

The earlier larger-corpus campaign recorded 1.545 s for model loading and 168.251 s for its first complete warmup/validation pass. That pass also performed geometry qualification, graph capture and reference checks. The later campaign recorded 40.9-44.3 s for its preparation/validation with compilation caches already present. These are distinct protocols and neither is a pure application cold-start measurement.

## Encoding is a separate measurement

The earlier 257-image RTX 5090 report records median warm encoding of **5.831 s** over three passes. The larger-corpus preparation took **147.100 s**, including source reading and writing/fsync, in one pass. The latter was archive preparation, not a tuned encoder comparison. Neither number should be substituted for the warm-decode timings above.

## Correctness and platform evidence

- The initial 257-image panel produced archives identical to the preserved reference when canonical entropy tables were used.
- Both GB10 ARM64 validation machines decoded the 257 RTX 5090 archives to the same reference RGB pixels.
- The larger-corpus campaign checked **2,506/2,506** reconstructed RGB hashes against an independent reference before and after timing.
- The later optimized profile retained integrity checks and reference equality. Its optimizations were qualified on RTX 5090; the entire larger corpus was not rerun on Spark.

Exact reconstruction refers to the reference NK8 decoded pixels. RGB remains lossy relative to the original image.

## Hardware and limitations

The timings belong to an RTX 5090 in an x86-64 WSL2 Ubuntu 24.04 environment with a prepared persistent decoder. They do not establish equivalent speed on GB10, CPUs, other GPUs or another corpus.

No matched JPEG/JPEG XL/AVIF comparison is published here. No perceptual acceptance study, physical-print qualification, universal quality guarantee or production storage-savings guarantee is claimed.

## Internal report provenance

The summary was transcribed from preserved release evidence:

- `NK_L8_RTX5090_2026-10-12.md`
- `NK_L8_test_reel_10Go_2026-10-13.md`
- `NK_L8_optimisations_process_2026-10-13.md`
- `NK_L8_compatibilite_Sparks_2026-10-13.md`
- NK8 1.0.0 release and reconstruction notes.

These filenames identify the source records; the private reports themselves are not included. Public numerical data are available in [results.json](results.json).
