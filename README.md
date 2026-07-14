# elizabeth-bennet-llm

Fine-tuning Llama-3.1-8B-Instruct to role-play as Elizabeth Bennet from *Pride and Prejudice*, evaluated against two prompt-engineered baselines across seven metrics (persona voice, hallucination resistance, plot recall, MCQ accuracy, fluency, coherency, and MBTI-consistency).

Full writeup: [blog post](#) <!-- add link once published -->

## What's here

| File | Purpose |
|---|---|
| `01_finetuning_elizabeth_bennet.ipynb` | QLoRA fine-tune of `unsloth/Llama-3.1-8B-Instruct-bnb-4bit` on ~100 Elizabeth-POV Q&A pairs, pushes the resulting adapter to the Hub. |
| `02_evaluation_pipeline.ipynb` | Runs all three arms (fine-tuned, zero-shot prompt, few-shot prompt) through the seven-metric eval harness and scores them with an LLM judge. |
| `data/eval_dataset.jsonl` | The seven-metric evaluation dataset, generated from the cleaned novel text. |
| `data/responses.json` | Raw model responses per arm per eval item. |
| `data/scores.json` | Per-item judge scores. |

Fine-tuned adapter: [KritiR/elizabeth-bennet-adapter](https://huggingface.co/KritiR/elizabeth-bennet-adapter)

## Running the notebooks

Both notebooks are written for Google Colab (free T4 tier is sufficient) and open directly via the Colab badge at the top of each file.

1. **`01_finetuning_elizabeth_bennet.ipynb`** — trains the adapter.
   - Requires a Hugging Face token (Colab secret `hf_token`) with write access, since it both reads the training Q&A dataset and pushes the resulting adapter to the Hub.
   - The training dataset (`KritiR/Elizabeth_question_answer`) is currently private — to rerun this notebook yourself, either request access or substitute your own Q&A dataset in the same `{question, answer}` JSONL format.
2. **`02_evaluation_pipeline.ipynb`** — runs the eval harness.
   - Requires an OpenAI API key (Colab secret `open_ai`) for the LLM-judge scoring step.
   - Loads the fine-tuned adapter above plus the two prompt-engineered baselines, and evaluates all three against `data/eval_dataset.jsonl`.

## Data provenance

The novel text (*Pride and Prejudice* by Jane Austen) is sourced from [Project Gutenberg](https://www.gutenberg.org/) and is in the public domain. The training Q&A pairs and evaluation dataset were generated from that text using Claude, then reviewed for quality.

## License

MIT — see [LICENSE](LICENSE).
