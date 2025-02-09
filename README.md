# Saving 77\% of the parameters in language models Technical Report
This repository contains the source code for the [Saving 77\% of the parameters in language models Technical Report (PDF)](https://www.researchgate.net/publication/360226228_Grouped_Pointwise_Convolutions_Reduce_Parameters_in_Convolutional_Neural_Networks).

## Abstract
This technical report demonstrates that large language models (LLMs) can maintain their learning capacity while reducing their non-embedding parameters by up to 77\%. We achieve this by adapting a parameter reduction technique originally developed for computer vision, replacing dense layers with an optimized subnetwork that contains grouped pointwise convolutions. Using Microsoft's phi-3-mini-4k-instruct as our baseline, we show that our optimized model (kphi-3) achieves comparable validation loss while using only 15-23\% of the original non-embedding parameters. All experiments were conducted on a single NVIDIA L2 GPU within a 3-day timeframe, supporting the democratization of AI research. Our findings suggest that current LLM architectures may be substantially overparameterized, opening possibilities for more efficient model training and deployment.

## Results

LaMini training results with the baseline and the optimized versions. From left to right: experiment label, model name, number of transformer decoder layers, intermediate dimensions, number of non-embedding parameters, training loss and validation loss.

| label | model | layers | interm. dims. | non-emb. params. | % | Train Loss | Val. Loss |
|:-----:|:------:|:-------:|:-------------:|:----------------:|:---:|:----------:|:----------:|
| JP47D54C | phi-3 | 2 | 8192 | 227M | | **1.08** | 1.58 |
| JP47D55C | kphi-3 | 2 | 9216 | **15M** | **15%** | 1.26 | 1.60 |
| JP47D56C | kphi-3 | 3 | 9216 | 23M | 23% | 1.21 | **1.57** |

## The Raw Results Folder
You can find all raw experiment files used for the technical report on the [raw](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw) folder.
