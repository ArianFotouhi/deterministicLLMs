---

## 📄 ** Using Open Source LLMs via Hugging Face for Deterministic or Stochastic Outputs**

```markdown
# 🧪 Controlling Determinism in Open-Source LLMs via Hugging Face

This guide shows how to run open-source LLMs with deterministic or non-deterministic behavior using the 🤗 Hugging Face Transformers library.

---

## 🚀 Open Source Models You Can Use

Here are some popular LLMs available on Hugging Face that support parameter control:

| Model Name                   | Size     | Determinism Support | Link |
|-----------------------------|----------|----------------------|------|
| Meta LLaMA 3                | 8B, 70B  | ✅ Full support       | [`meta-llama/Meta-Llama-3-8B`](https://huggingface.co/meta-llama/Meta-Llama-3-8B) |
| Mistral                     | 7B       | ✅ Full support       | [`mistralai/Mistral-7B`](https://huggingface.co/mistralai/Mistral-7B) |
| Falcon                      | 7B, 40B  | ✅ Full support       | [`tiiuae/falcon-7b`](https://huggingface.co/tiiuae/falcon-7b) |
| GPT-NeoX / Pythia           | Various  | ✅ Full support       | [`EleutherAI/gpt-neox-20b`](https://huggingface.co/EleutherAI/gpt-neox-20b) |

---

## ⚙️ Setup

```bash
pip install transformers torch
````

---

## 🧩 Deterministic Generation

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch
import numpy as np
import random

# Fix random seed
seed = 42
torch.manual_seed(seed)
random.seed(seed)
np.random.seed(seed)
torch.use_deterministic_algorithms(True)

# Load model
model_id = "meta-llama/Meta-Llama-3-8B"
model = AutoModelForCausalLM.from_pretrained(model_id)
tokenizer = AutoTokenizer.from_pretrained(model_id)

# Prepare input
prompt = "The theory of relativity is"
inputs = tokenizer(prompt, return_tensors="pt")

# Deterministic generation
output = model.generate(
    **inputs,
    do_sample=False,
    temperature=0.0,
    top_k=0,
    top_p=1.0,
    max_new_tokens=50
)

print(tokenizer.decode(output[0], skip_special_tokens=True))
```

---

## 🎲 Non-Deterministic Generation

```python
output = model.generate(
    **inputs,
    do_sample=True,
    temperature=0.9,
    top_k=40,
    top_p=0.95,
    max_new_tokens=50
)
```

---

