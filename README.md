# NK8

**GPU-accelerated learned image compression for high-volume image workflows.**

NK8 combines a learned image codec with a specialized runtime designed to balance compressed size, reconstruction quality and processing speed. It builds on NACRE and CompressAI, with custom native components and GPU optimizations.

This is the public overview of **NK8 1.0.0**. The encoder, decoder implementation, custom kernels, model artifacts and internal tooling remain private. This repository currently contains documentation and a summary of recorded results; it is not an installable distribution.

## Recorded results

The following measurements come from internal RTX 5090 reports dated **13 October 2026**. They are recorded results, not independently reproduced public benchmarks.

| Larger validation corpus | Recorded result |
|---|---:|
| Distinct images | 2,506 |
| Source files | 9.848 GB |
| Pixels | 6.903 billion |
| Complete NK8 archives | 1.393 GB |
| Archive size relative to source files | 14.14% |
| Reduction relative to source files | 85.86% |
| Mean RGB8 PSNR | 42.024 dB |
| Later optimized warm decode | 16.483 s |
| Derived pixel throughput | Approximately 419 MP/s |

GB means decimal gigabytes. The size and quality figures describe this corpus. The later decode result uses the same corpus and archives with an optimized execution profile. Quality was evaluated separately from timing.

The retained runtime also records **1.721952 s** for a separately prepared **257-image / 714.877-megapixel** panel. These preparation states are distinct and their timings should not be mixed.

**The headline times measure warm decoding into RGB8 in host memory.** They include archive reading, integrity checks, CPU entropy decoding and GPU reconstruction. Model loading, initial preparation, final quality/hash comparisons and PNG/JPEG output are outside the timer. They are not encoding times or cold-start application latencies.

See [Results and measurement scope](docs/RESULTS.md) for the full context.

## What NK8 does

1. Transforms image pixels into compact learned features.
2. Quantizes and entropy-codes those features into versioned archives.
3. Decodes the features and reconstructs the image through a neural network.
4. Preserves supported alpha and original ICC, EXIF and XMP metadata through the archive path.

The runtime uses custom native components, fused GPU operations, persistent model state, overlapping execution and bounded work queues. Its purpose is to make learned compression practical for repeated image processing.

NK8 is **lossy for RGB**. Exact decoding means reproducing the qualified NK8 reconstruction; it does not mean recovering the original RGB pixels without loss.

## Intended use

NK8 targets controlled image pipelines where storage footprint, image fidelity and sustained processing throughput all matter. Potential applications include image libraries, internal asset services and storage-oriented processing systems.

Whether reconstructed images can replace originals is an application decision. The published metrics do not constitute visual acceptance for every image, physical-print validation or a guarantee of savings on another archive.

## Platform and compatibility

The preserved release supports qualified **8-bit RGB/RGBA** workflows on Linux with NVIDIA GPUs. Recorded restoration and compatibility checks cover:

- RTX 5090, x86-64, Ubuntu 24.04 under WSL2.
- NVIDIA GB10 / DGX Spark, ARM64, Ubuntu 24.04.

The optimized decode timings above belong to the RTX 5090. They do not describe Spark performance. A 257-image compatibility panel reconstructed identical reference pixels across the qualified platforms.

See [Architecture](docs/ARCHITECTURE.md) and [FAQ](docs/FAQ.md).

## Project status and access

NK8 is under active evaluation. Public binaries, source code and model downloads are not included in this repository. No release date or future open-source commitment is announced.

For evaluation or licensing inquiries, open an issue with a **non-confidential** description of the intended workflow, hardware, image types and throughput needs. Private images, credentials and company-sensitive information should not be posted in public issues.

## Comparison with other codecs

The relevant comparison is the joint trade-off between **speed, compressed size and acceptable image quality** under equivalent conditions. This repository does not claim a matched victory over JPEG, JPEG XL, AVIF or another learned codec. A quality score on one corpus cannot establish that conclusion.

## Attribution and rights

NK8 was developed by **Maxime ([@Niltark](https://github.com/Niltark))**. It builds on work in NACRE and the [CompressAI research framework](https://github.com/InterDigitalInc/CompressAI).

Public documentation does not make the NK8 implementation open source. See [Rights and availability](RIGHTS.md).
