# Saving 77\% of the Parameters in Large Language Models Technical Report
This repository contains the source code for the [Saving 77\% of the Parameters in Large Language Models Technical Report (PDF)](https://www.researchgate.net/publication/388835829_SAVING_77_OF_THE_PARAMETERS_IN_LARGE_LANGUAGE_MODELS_TECHNICAL_REPORT).

## Abstract
This technical report demonstrates that large language models (LLMs) can maintain their learning capacity while reducing their non-embedding parameters by up to 77\%. We achieve this by adapting a parameter reduction technique originally developed for computer vision, replacing dense layers with an optimized subnetwork that contains grouped pointwise convolutions. Using Microsoft's phi-3-mini-4k-instruct as our baseline, we show that our optimized model (kphi-3) achieves comparable validation loss while using only 15-23\% of the original non-embedding parameters. All experiments were conducted on a single NVIDIA L4 GPU within a 3-day timeframe, supporting the democratization of AI research. Our findings suggest that current LLM architectures may be substantially overparameterized, opening possibilities for more efficient model training and deployment.

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
| [JP47D55C](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw/JP47D55C_kphi3_2T.ipynb) | kphi-3 | 2 | 9216 | [**35M**](https://huggingface.co/schuler/experimental-JP47D55C) | **15%** | 1.26 | 1.60 |
| [JP47D56C](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw/JP47D56C_kphi3_3T.ipynb) | kphi-3 | 3 | 9216 | [53M](https://huggingface.co/schuler/experimental-JP47D56C) | 23% | 1.21 | **1.57** |

## Quick Links
- [📄 Full Technical Report (PDF)](https://www.researchgate.net/publication/388835829_SAVING_77_OF_THE_PARAMETERS_IN_LARGE_LANGUAGE_MODELS_TECHNICAL_REPORT)
- [🤗 Model Checkpoints on HuggingFace](https://huggingface.co/schuler/)
- [📊 Raw Experiment Files](https://github.com/joaopauloschuler/less-parameters-llm/tree/main/raw)

## Repository Structure
- /raw: Contains all raw experiment files and notebooks. Each experiment is documented in separate notebooks (JP47D54C, JP47D55C, JP47D56C).
- /src: Source code of the optimized model.

## Usage:
```
!pip install -q -U transformers
!pip install -q -U accelerate
!pip install -q -U flash-attn --no-build-isolation

from transformers import AutoTokenizer, AutoModelForCausalLM, GenerationConfig, pipeline
from transformers import LlamaTokenizer
import torch

REPO_NAME = 'schuler/experimental-JP47D56C'

def load_model(local_repo_name):
    tokenizer = LlamaTokenizer.from_pretrained(local_repo_name, trust_remote_code=True)
    generator_conf = GenerationConfig.from_pretrained(local_repo_name)
    model = AutoModelForCausalLM.from_pretrained(local_repo_name, trust_remote_code=True, attn_implementation="flash_attention_2", torch_dtype=torch.float16)
    model.to('cuda')
    return tokenizer, generator_conf, model

tokenizer, generator_conf, model = load_model(REPO_NAME)
global_error = ''
try:
  generator = pipeline("text-generation", model=model, tokenizer=tokenizer)
except Exception as e:
  global_error =  f"Failed to load model: {str(e)}"

def PrintTest(str):
  print(generator(str, max_new_tokens=256, do_sample=True, top_p=0.5, repetition_penalty=1.2))

PrintTest(f"<|user|>\nHello\n<|end|>\n<|assistant|>\n")
PrintTest(f"<|user|>Hello\n<|end|><|assistant|>")
PrintTest(f"<|user|>\nWhat is the human body?\n<|end|>\n<|assistant|>\n")
PrintTest(f"<|user|>What is the human body?\n<|end|><|assistant|>")
PrintTest(f"<|user|>What is biology?\n<|end|><|assistant|>")
PrintTest(f"<|user|>Can you comment about democracy?\n<|end|><|assistant|>")
PrintTest(f"<|user|>Can you provide detailed comments about the concept of democracy?\n<|end|><|assistant|>")
PrintTest(f"<|user|>Please give me a detailed description of the python computer language.\n<|end|><|assistant|>")
PrintTest(f"<|user|>If you had a difficult task to complete, how would you complete it?\n<|end|><|assistant|>")
PrintTest(f"<|user|>Before replying to my question, I would like you to provide two candidate solutions and do some reflexion about these solution. Then, you'll pick the best as the final reply. What is best: eating healthy or eating economically?\n<|end|><|assistant|>")
```
## Output Examples
```
[{'generated_text': '<|user|>\nHello\n<|end|>\n<|assistant|>\n Please provide me with the instructions you would like me to respond concisely. <|end|>'}]
[{'generated_text': "<|user|>I have a question to you. Before you reply to my question, I need to to comment details about the subject without trying to directly reply to the question. As an example, if I ask you about modern architecture, you'll start describing what is architecture without specifics about modernism. Then, later you can reply to the question. Therefore, here we go with the question: what is ancient history?<|end|><|assistant|> The question is asking for the purpose of being knowledgeable in modern architecture. <|end|>"}]
[{'generated_text': "<|user|>Hello\n<|end|><|assistant|> I'm sorry, but I need more information to answer that question. Which particular phrase are you referring to? Please provide me with more context or specify which article you are referring to. <|end|>"}]
[{'generated_text': '<|user|>\nWhat is the human body?\n<|end|>\n<|assistant|>\n The human body is a complex organ system that includes all the internal organs, such as the heart, lungs, liver, and kidneys. <|end|>'}]
[{'generated_text': '<|user|>What is the human body?\n<|end|><|assistant|> The human body is a complex organ system that includes all of its functions, including:\n\n1. Bones (such as the brain and spinal cord)\n2. Cells and tissues (like organs and blood vessels)\n3. Lungs and heart valve\n4. Brain-emitting diet\n5. Skin digestion and absorption of nutrients\n6. Kidneys and regulate fluid balance\n7. Glycogen stores and eliminates insulin\n8. Cells and lymphoma\n9. Muscle contraction and relaxation\n10. Cardio and other physical activities. <|end|>'}]
[{'generated_text': '<|user|>What is biology?\n<|end|><|assistant|> Biology refers to the study of living organisms and their interactions with each other. It involves studying the physical, chemical, biological, genetic, and behavior of an organism or system in which it has a unique set of characteristics that define them. Biologists use various techniques such as molecular dynamics, gene transfer, and DNA sequencing to create new species. <|end|>'}]
[{'generated_text': "<|user|>Can you comment about democracy?\n<|end|><|assistant|> Yes, I can't help but notice that in a way that is not representative of all political parties or institutions. Could you please provide me with more information on what specifically you are looking for? <|end|>"}]
[{'generated_text': "<|user|>Can you provide detailed comments about the concept of democracy?\n<|end|><|assistant|> Democracy is a form of government where power is held by the people, either directly or through elected representatives. In a democracy, citizens have the ability to participate in decision-making processes and hold accountable those who do not receive it. The way in which citizens vote varies depending on their participation, but generally speaking, they are able to make informed decisions that align with one's values and priorities. Ultimately, democracy promotes equality and fairness, while majority wishes for equal representation and participation based on individual needs and preferences. <|end|>"}]
[{'generated_text': '<|user|>Please give me a detailed description of the python computer language.\n<|end|><|assistant|> The instructions provided do not specify what you want me to provide as I am an artificial intelligence language model and do not specify any specific type of computer language. Please provide more details or clarify your request so that I can assist you better. <|end|>'}]
[{'generated_text': '<|user|>If you had a difficult task to complete, how would you complete it?\n<|end|><|assistant|> You could complete the task by providing clear instructions and guidance on completing the task. <|end|>'}]
[{'generated_text': "<|user|>Before replying to my question, I would like you to provide two candidate solutions and do some reflexion about these solution. Then, you'll pick the best as the final reply. What is best: eating healthy or eating economically?\n<|end|><|assistant|> Eating healthy or eating is best as it helps with weight management. <|end|>"}]
```

## Citing this Work 
```
@article{SchulerRojas_2025,
  title={Saving 77% of the Parameters in Large Language Models Technical Report},
  url={https://www.researchgate.net/publication/388835829_SAVING_77_OF_THE_PARAMETERS_IN_LARGE_LANGUAGE_MODELS_TECHNICAL_REPORT},
  author={Schwarz Schuler, Joao Paulo and Rojas Gómez, Alejandra},
  year={2025}}
```

