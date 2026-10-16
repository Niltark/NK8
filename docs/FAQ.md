# Frequently asked questions

## Is NK8 open source?

No. This repository publishes an overview and recorded result summaries. Implementation code, custom kernels, model artifacts and canonical entropy tables remain private. See [Rights and availability](../RIGHTS.md).

## Can I download or run it?

No public binary or model release is included at this stage. Evaluation inquiries can be raised in the repository's public issues without sharing confidential material. Availability and any evaluation terms must be agreed separately.

## Does NK8 use AI to invent missing image content?

NK8 uses learned analysis and reconstruction networks in an image-compression pipeline. It is not a text-prompt image-generation workflow. As with other lossy codecs, reconstruction can alter or remove details; fidelity needs to be assessed for the intended use.

## Is it lossless?

RGB compression is lossy. The exactness claim concerns reproducing the qualified decoder's output pixels. Supported alpha and metadata handling have their own integrity checks.

## Why use a private implementation?

This publication introduces the project while preserving options for evaluation, development and commercial licensing. It does not commit to a particular future source-release model.

## Is it better than JPEG XL or AVIF?

No equivalent-condition comparison has been established by the evidence published here. NK8's goal is a useful combination of speed, size and quality. A defensible comparison must use the same images, suitable settings, clear timing boundaries and an appropriate quality assessment.

## Does it reduce every archive by 85.86%?

No. That is the measured source-file reduction for the specific larger corpus in [the results](RESULTS.md). Other archives can behave differently. Existing file compression, image content, bit depth and fidelity requirements all matter.

## Can it replace original production masters?

That depends on the workflow's fidelity and retention requirements. The current contract supports 8-bit RGB/RGBA and lossy RGB. Preserved metadata and a high average PSNR do not establish that it meets every master-image requirement.

## Can other machines recover the files?

Reference decoding has been validated across the qualified RTX 5090 x86-64 and GB10 ARM64 environments on a 257-image panel. That is a specific compatibility result. A universal CPU decoder and independent public recovery implementation are not currently supplied.
