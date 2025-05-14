# 🤖 Determinism in Large Language Models: Theory and Parameters

Large Language Models (LLMs) are typically **non-deterministic** during inference, meaning the same input prompt can yield different outputs. This is due to the use of **probabilistic sampling techniques** during text generation.

This document explains **which parameters control determinism**, their theoretical impact, and how you can manipulate them for **predictable or diverse results**.

---

## 🧠 What Is Determinism in LLMs?

**Determinism** means that, given the same input and environment, the model always produces the exact same output.

---

## 🎚️ Key Parameters That Affect Determinism

| Parameter     | Description | Deterministic? | Typical Range | Example |
|--------------|-------------|----------------|---------------|---------|
| `do_sample`  | Enables random sampling of tokens | ❌ | `True` / `False` | `do_sample=True` uses randomness |
| `temperature`| Controls randomness in token probabilities | ❌ | `0.0 - 1.5` | `temperature=1.0` = baseline, `0.0` = greedy |
| `top_k`      | Samples from top-k most likely tokens | ❌ | `0 - 100` | `top_k=50` restricts to 50 choices |
| `top_p`      | Samples from smallest set with cumulative prob ≥ p | ❌ | `0.0 - 1.0` | `top_p=0.9` covers 90% of likely tokens |
| `seed`       | Fixes the random number generator | ✅ | any int | `seed=42` gives repeatability |
| `beam_search`| Searches multiple sequences simultaneously | ⚠️ | beam width ≥ 1 | `num_beams=5` allows diversity |
| `greedy`     | Always chooses most likely next token | ✅ | enabled with `do_sample=False`, `temperature=0` | Used in deterministic runs |

---

## 🔄 Examples

### 🎯 Deterministic Output

```python
outputs = model.generate(
    input_ids,
    do_sample=False,
    temperature=0.0,
    top_k=0,
    top_p=1.0
)
````

### 🎲 Non-Deterministic Output

```python
outputs = model.generate(
    input_ids,
    do_sample=True,
    temperature=0.9,
    top_k=50,
    top_p=0.9
)
```

---

## ⚖️ Tradeoffs: Determinism vs Creativity

| Feature         | Deterministic Mode              | Sampling Mode              |
| --------------- | ------------------------------- | -------------------------- |
| Reproducibility | ✅ Yes                           | ❌ No                       |
| Creativity      | ❌ Limited                       | ✅ High                     |
| Use case        | Testing, evaluation, production | Story generation, ideation |

---

## ✅ TL;DR: Making LLMs Deterministic

1. Set `do_sample=False`
2. Set `temperature=0.0`
3. Set `top_k=0`
4. Set `top_p=1.0`
5. Fix the seed (if supported)
6. Use same hardware/software for full reproducibility

---




## 📌 Notes

* Deterministic results require:

  * Fixed model
  * Fixed parameters
  * Fixed random seed
  * Same hardware (optional but recommended)
* Not all APIs expose seed control (e.g. OpenAI’s GPT)

---

## ✅ Summary

| Goal                  | Settings Needed                                                 |
| --------------------- | --------------------------------------------------------------- |
| **Deterministic**     | `do_sample=False`, `temperature=0`, `top_k=0`, `top_p=1.0`      |
| **Non-Deterministic** | `do_sample=True`, `temperature > 0`, `top_k > 0`, `top_p < 1.0` |

---

## 📎 License

This document is provided under the MIT License.

