---
license: apache-2.0
task_categories:
- text-generation
- question-answering
language:
- en
---

# MATH (minus MATH-500)

This dataset is derived from the original MATH dataset by Hendrycks et al.
([qwedsacf/competition_math](https://huggingface.co/datasets/qwedsacf/competition_math)) with all problems from the MATH-500 benchmark set removed.

You can also find this dataset on the Hugging Face Hub at [rasbt/math_full_minus_math500](https://huggingface.co/datasets/rasbt/math_full_minus_math500/tree/main)

&nbsp;
## Construction

- Source: 12,500 problems from the MATH dataset by Hendrycks et al. ([qwedsacf/competition_math](https://huggingface.co/datasets/qwedsacf/competition_math))
- Benchmark held out: 500 problems from the MATH-500 dataset ([HuggingFaceH4/MATH-500](https://huggingface.co/datasets/HuggingFaceH4/MATH-500))
- Matching criterion: exact match on the `problem` field (see https://github.com/rasbt/math_full_minus_math500 for code to prepare the dataset)
- Remaining size: 12,000 problems
- Additionally, an `"answer"` field was added to the entries in the MATH dataset that contains the short answer similar to MATH-500

&nbsp;
## Fields

Each example contains:
- `problem`: math problem statement
- `solution`: full worked solution
- `answer`: extracted final answer using `extract_final_candidate` function from the [reasoning-from-scratch](https://github.com/rasbt/reasoning-from-scratch) Python package (matches those in the MATH-500 dataset)
- `subject`: math subject
- `level`: difficulty level
- `unique_id`: original problem identifier

&nbsp;
## Intended use

This dataset is intended for training only.
Evaluation should be performed on MATH-500, which is excluded.

&nbsp;
## JSON Usage

```python
import json
import requests
from pathlib import Path

local_path = Path("math500_test.json")
url = (
    "https://raw.githubusercontent.com/rasbt/math_full_minus_math500/"
    "main/math_full_minus_math500.json"
)

if local_path.exists():
    with local_path.open("r", encoding="utf-8") as f:
        math_data = json.load(f)
else:
    r = requests.get(url, timeout=30)
    r.raise_for_status()
    math_data = r.json()

    # save locally
    with local_path.open("w", encoding="utf-8") as f:
        json.dump(math_data, f, ensure_ascii=False, indent=2)

print("Number of entries:", len(math_data))
```

&nbsp;
## Hugging Face `datasets` Usage

```python
from datasets import load_dataset

dataset = load_dataset(
    "rasbt/math_full_minus_math500",
    split="train"
)

print(len(dataset))
print(dataset[0].keys())
```
