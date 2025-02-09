# Saving 77\% of the parameters in language models Technical Report
This repository contains the source code for the [Saving 77\% of the parameters in language models Technical Report (PDF)](https://www.researchgate.net/publication/360226228_Grouped_Pointwise_Convolutions_Reduce_Parameters_in_Convolutional_Neural_Networks).

## Abstract
This technical report demonstrates that large language models (LLMs) can maintain their learning capacity while reducing their non-embedding parameters by up to 77\%. We achieve this by adapting a parameter reduction technique originally developed for computer vision, replacing dense layers with an optimized subnetwork that contains grouped pointwise convolutions. Using Microsoft's phi-3-mini-4k-instruct as our baseline, we show that our optimized model (kphi-3) achieves comparable validation loss while using only 15-23\% of the original non-embedding parameters. All experiments were conducted on a single NVIDIA L2 GPU within a 3-day timeframe, supporting the democratization of AI research. Our findings suggest that current LLM architectures may be substantially overparameterized, opening possibilities for more efficient model training and deployment.

## Results


## The Raw Results Folder
You can find all raw experiment files used for the technical report on the [raw](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw) folder.
