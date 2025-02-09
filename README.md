# Saving 77\% of the parameters in language models Technical Report
This repository contains the source code for the [Saving 77\% of the parameters in language models Technical Report (PDF)](https://www.researchgate.net/publication/360226228_Grouped_Pointwise_Convolutions_Reduce_Parameters_in_Convolutional_Neural_Networks).

## Abstract
This technical report demonstrates that large language models (LLMs) can maintain their learning capacity while reducing their non-embedding parameters by up to 77\%. We achieve this by adapting a parameter reduction technique originally developed for computer vision, replacing dense layers with an optimized subnetwork that contains grouped pointwise convolutions. Using Microsoft's phi-3-mini-4k-instruct as our baseline, we show that our optimized model (kphi-3) achieves comparable validation loss while using only 15-23\% of the original non-embedding parameters. All experiments were conducted on a single NVIDIA L2 GPU within a 3-day timeframe, supporting the democratization of AI research. Our findings suggest that current LLM architectures may be substantially overparameterized, opening possibilities for more efficient model training and deployment.

## Key Findings
- Achieved 77% parameter reduction while maintaining model performance.
- Demonstrated better generalization in optimized models.
- Improved output quality in qualitative testing.

## Implementation Details
- Base Model: Microsoft's [phi-3-mini-4k-instruct](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct).
- Training Dataset: [LaMini](https://huggingface.co/datasets/MBZUAI/LaMini-instruction).
- Architecture: Modified transformer decoder with grouped pointwise convolutions.

## Results
The following table shows LaMini training results with the baseline and the optimized versions. From left to right: experiment label, model name, number of transformer decoder layers, intermediate dimensions, number of non-embedding parameters, training loss and validation loss.

| label | model | layers | interm. dims. | non-emb. params. | % | Train Loss | Val. Loss |
|:-----:|:------:|:-------:|:-------------:|:----------------:|:---:|:----------:|:----------:|
| [JP47D54C](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw/JP47D54C_Baseline_2T.ipynb) | phi-3 | 2 | 8192 | [227M](https://huggingface.co/schuler/experimental-JP47D54C) | | **1.08** | 1.58 |
| [JP47D55C](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw/JP47D55C_kphi3_2T.ipynb) | kphi-3 | 2 | 9216 | [**15M**](https://huggingface.co/schuler/experimental-JP47D55C) | **15%** | 1.26 | 1.60 |
| [JP47D56C](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw/JP47D56C_kphi3_3T.ipynb) | kphi-3 | 3 | 9216 | [23M](https://huggingface.co/schuler/experimental-JP47D56C) | 23% | 1.21 | **1.57** |

## Quick Links
- [📄 Full Technical Report (PDF)](https://www.researchgate.net/publication/360226228_Grouped_Pointwise_Convolutions_Reduce_Parameters_in_Convolutional_Neural_Networks)
- [🤗 Model Checkpoints on HuggingFace](https://huggingface.co/schuler/)
- [📊 Raw Experiment Files](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw)

## Repository Structure
- /raw: Contains all raw experiment files and notebooks. Each experiment is documented in separate notebooks (JP47D54C, JP47D55C, JP47D56C).
- /src: Source code of the optimized model.
