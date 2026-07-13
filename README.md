# SILC-TSE: Inference Demo

## Overview

SILC-TSE is a lightweight causal target speaker extraction model. Given a speech mixture waveform and a reference waveform from the target speaker, the model estimates the target-speaker waveform from the mixture.

This repository is an inference-only demo for ONNX inference verification and qualitative listening evaluation. It does not include training code, the complete PyTorch implementation, data preparation scripts, experimental configurations, ablation scripts, or the complete manuscript reproduction pipeline.

## Model Interface

| Item | Description |
|---|---|
| `mixture` | Input speech mixture waveform |
| `reference` | Reference waveform from the target speaker |
| `estimate` | Output estimate waveform for the target speaker |
| Sampling rate | 8 kHz |
| Duration | 4 seconds |
| Samples | 32000 |
| Input shape | `[1, 1, 32000]` |
| Output shape | `[1, 1, 32000]` |
| Batch size | 1 |
| Runtime provider | `CPUExecutionProvider` |

Stereo input is converted to mono. Audio is resampled to 8 kHz. Audio shorter than 4 seconds is zero-padded. Audio longer than 4 seconds is deterministically truncated from the beginning.

## Repository Structure

```text
checkpoints/
  silc_tse_demo.onnx
examples/
  showcase_01/
  showcase_02/
  showcase_03/
outputs/
  .gitkeep
.gitignore
inference.py
requirements.txt
README.md
```

## Installation

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows:

```bat
.venv\Scripts\activate
```

Activate it on Linux/macOS:

```bash
source .venv/bin/activate
```

Install the inference dependencies:

```bash
pip install -r requirements.txt
```

## Run Inference

Extract speaker 1 from the first showcase example:

```bash
python inference.py \
  --mixture examples/showcase_01/mixture.wav \
  --reference examples/showcase_01/reference_s1.wav \
  --model checkpoints/silc_tse_demo.onnx \
  --output outputs/showcase_01_s1.wav
```

Extract speaker 2 from the first showcase example:

```bash
python inference.py \
  --mixture examples/showcase_01/mixture.wav \
  --reference examples/showcase_01/reference_s2.wav \
  --model checkpoints/silc_tse_demo.onnx \
  --output outputs/showcase_01_s2.wav
```

## Demo Audio Examples

Three curated qualitative examples are provided. Each example contains one mixture, two clean source signals, two target-speaker reference utterances, and two pre-generated SILC-TSE outputs.

### Showcase 01

| File | Description |
|---|---|
| [mixture.wav](examples/showcase_01/mixture.wav) | Input speech mixture |
| [clean_s1.wav](examples/showcase_01/clean_s1.wav) | Clean source signal for speaker 1 |
| [clean_s2.wav](examples/showcase_01/clean_s2.wav) | Clean source signal for speaker 2 |
| [reference_s1.wav](examples/showcase_01/reference_s1.wav) | Reference utterance for speaker 1 |
| [reference_s2.wav](examples/showcase_01/reference_s2.wav) | Reference utterance for speaker 2 |
| [estimate_s1.wav](examples/showcase_01/estimate_s1.wav) | SILC-TSE output using `reference_s1.wav` |
| [estimate_s2.wav](examples/showcase_01/estimate_s2.wav) | SILC-TSE output using `reference_s2.wav` |

### Showcase 02

| File | Description |
|---|---|
| [mixture.wav](examples/showcase_02/mixture.wav) | Input speech mixture |
| [clean_s1.wav](examples/showcase_02/clean_s1.wav) | Clean source signal for speaker 1 |
| [clean_s2.wav](examples/showcase_02/clean_s2.wav) | Clean source signal for speaker 2 |
| [reference_s1.wav](examples/showcase_02/reference_s1.wav) | Reference utterance for speaker 1 |
| [reference_s2.wav](examples/showcase_02/reference_s2.wav) | Reference utterance for speaker 2 |
| [estimate_s1.wav](examples/showcase_02/estimate_s1.wav) | SILC-TSE output using `reference_s1.wav` |
| [estimate_s2.wav](examples/showcase_02/estimate_s2.wav) | SILC-TSE output using `reference_s2.wav` |

### Showcase 03

| File | Description |
|---|---|
| [mixture.wav](examples/showcase_03/mixture.wav) | Input speech mixture |
| [clean_s1.wav](examples/showcase_03/clean_s1.wav) | Clean source signal for speaker 1 |
| [clean_s2.wav](examples/showcase_03/clean_s2.wav) | Clean source signal for speaker 2 |
| [reference_s1.wav](examples/showcase_03/reference_s1.wav) | Reference utterance for speaker 1 |
| [reference_s2.wav](examples/showcase_03/reference_s2.wav) | Reference utterance for speaker 2 |
| [estimate_s1.wav](examples/showcase_03/estimate_s1.wav) | SILC-TSE output using `reference_s1.wav` |
| [estimate_s2.wav](examples/showcase_03/estimate_s2.wav) | SILC-TSE output using `reference_s2.wav` |

## ONNX Validation

The ONNX model was compared with the original PyTorch implementation on 100 real test samples.

Maximum task-level differences:

| Metric | Maximum difference |
|---|---:|
| SI-SNR | 0.00644 dB |
| PESQ | 0.000260 |
| STOI | 0.000139 |

These differences are negligible for inference verification and qualitative listening evaluation. The validation is intended to support inference verification and qualitative listening evaluation, not to claim exact equality with the original PyTorch implementation.

## Scope and Limitations

- This demo supports fixed 4-second input only.
- Batch size is fixed to 1.
- Arbitrary-length input is unsupported.
- Streaming state input is not provided.
- Training and fine-tuning are unsupported.
- Complete manuscript experiments cannot be reproduced from this repository.
- Minor numerical differences may occur across ONNX Runtime versions and hardware.

## Demo Audio Notice

The three audio examples are curated qualitative examples. They are not random or unbiased test samples, and they do not replace the quantitative results reported in the manuscript.

The examples are WSJ0/WSJ0-2mix-derived limited excerpts included solely for non-commercial research demonstration during anonymous peer review. The original speech corpus remains subject to the terms of its original dataset license. This repository does not grant additional rights to reuse or redistribute the original dataset content.

The full WSJ0 and WSJ0-2mix datasets, dataset partitions, file lists, and data preparation scripts are not included.

## Anonymous Review

Author names, affiliations, contact information, and other identifying details are intentionally omitted. This anonymous review demo does not contain author identity information.

