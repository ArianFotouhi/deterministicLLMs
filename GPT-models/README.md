# Deterministic vs. Non-Deterministic Behavior in OpenAI Chat Completions

This project demonstrates the effect of various parameters (`temperature`, `top_p`, and `seed`) on the behavior of a language model using the OpenAI API. It uses the model `gpt-4.1-nano` to answer a question about extracting whey protein from milk and analyzes how deterministic or variable the responses are under different settings.

## Requirements

- Python 3.7+
- `openai` Python library

Install dependencies:
```bash
pip install openai
````

## Usage

### Prompt

All examples use the same base prompt:

```python
messages = [
    {"role": "system", "content": "Answer the questions in three sentences."},
    {"role": "user", "content": "How to get whey protein from milk?"}
]
```

### 1. Deterministic LLM

This configuration forces the model to be as deterministic as possible:

```python
temperature = 0
top_p = 1
seed = 0
```

#### Output

The model returns **identical** responses for each run, confirming determinism.

---

### 2. Non-Deterministic LLM (by `top_p`)

This configuration introduces randomness by reducing `top_p`:

```python
temperature = 0
top_p = 0.6
seed = 0
```

#### Output

The responses are similar but show slight variability in wording due to narrower sampling from the probability distribution.

---

### 3. Non-Deterministic LLM (by `seed`)

Changing the `seed` while keeping other parameters constant:

```python
temperature = 0
top_p = 1
seed = i  # varies in each iteration
```

#### Output

Minor differences appear across responses, highlighting that even with deterministic settings, changing the seed can impact output subtly.

---

### 4. Non-Deterministic LLM (by `temperature`)

Varying the `temperature` parameter:

```python
temperature = i  # 0 and 1
top_p = 1
seed = 0
```

#### Output

Higher temperature leads to more diverse and creative answers, while `temperature = 0` keeps output consistent.

---

## Summary

| Config Type               | Variable        | Output Behavior    |
| ------------------------- | --------------- | ------------------ |
| Deterministic             | None (fixed)    | Identical          |
| Non-Deterministic (Top-p) | `top_p=0.6`     | Slightly varied    |
| Non-Deterministic (Seed)  | `seed=i`        | Slightly varied    |
| Non-Deterministic (Temp)  | `temperature=i` | Varied or creative |

## License

This project is provided for educational and experimental purposes. Check OpenAI’s usage policies before deploying in production.

## Author

Generated and tested using OpenAI API with `gpt-4.1-nano`.



