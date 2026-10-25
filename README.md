# Prompt Engineering Lab (VVIT)

Lab record for the **Prompt Engineering** course (IV year, CSM-A), Vasireddy Venkatadri Institute of Technology.

All 16 experiments are in a single notebook: [`PromptEngineering_IV_CSM_A_23BQ1A4201_Exps.ipynb`](PromptEngineering_IV_CSM_A_23BQ1A4201_Exps.ipynb).
Each experiment follows the same format: **Aim**, **Description**, **Code**.

The final lab record in the college format (border, header, footer, and per experiment: ExpNo, Aim, Description, Program, Output) is available as a PDF: [`pe_record_23BQ1A4201.pdf`](pe_record_23BQ1A4201.pdf) (54 pages). Its LaTeX source is in [`latex-record/`](latex-record/).

## Experiments

| # | Topic | Model / Tools |
|---|-------|---------------|
| 1 | Basic text generation with a pre-trained model | `distilgpt2` |
| 2 | Baseline prompt vs. enhanced prompt | `distilgpt2` |
| 3 | Iterative prompt refinement (summarization) | `distilgpt2` |
| 4 | Prompt clarity and specificity (vague vs. refined) | `distilgpt2` |
| 5 | Zero-shot vs. few-shot prompting (sentiment classification) | `distilgpt2` |
| 6 | Normal vs. role-based vs. role + negative prompting | `distilgpt2` |
| 7 | Iterative refinement cycles for summarizing a technical topic | `distilgpt2` |
| 8 | Structured output: bullet list and Markdown table, with verification | `distilgpt2` |
| 9 | JSON generation and validation with a parser | `distilgpt2` |
| 10 | Direct answer vs. zero-shot Chain-of-Thought vs. task decomposition | `distilgpt2` |
| 11 | Simple LangChain Expression Language (LCEL) chain | LangChain + Ollama (`llama3.2`) |
| 12 | RAG data indexing: chunking, embeddings, in-memory vector store | `tiktoken`, `all-MiniLM-L6-v2` |
| 13 | Complete RAG chain: retrieval + answer generation | `all-MiniLM-L6-v2`, Google Gemini (`gemini-flash-latest`) |
| 14 | LLM agent with a calculator tool (function calling) | Google Gemini (`gemini-flash-latest`) |
| 15 | Multimodal: image generation, captioning, prompt-caption alignment | Gemini image model (Nano Banana), `gemini-flash-latest` |
| 16 | Prompt evaluation, LLM-as-a-judge, prompt-injection testing | Google Gemini (`gemini-flash-latest`) |

## What you need

| Experiments | Requirement |
|-------------|-------------|
| 1-10 | Python packages only (`transformers`, `torch`). Models are downloaded automatically from Hugging Face. |
| 11 | [Ollama](https://ollama.com) installed and running, with the model pulled: `ollama pull llama3.2` |
| 12 | Python packages only (`sentence-transformers`, `tiktoken`) |
| 13-16 | A Google Gemini API key in the `GEMINI_API_KEY` environment variable. Get one at [Google AI Studio](https://aistudio.google.com/apikey). The free tier has rate limits, and image generation (Experiment 15) may need billing enabled. |

Python 3.10 or newer is recommended.

## Setup

### Option A: Run locally

```bash
git clone https://github.com/AdhimulamBhargavSaiViswanath-05/prompt-engineering-lab-vvit.git
cd prompt-engineering-lab-vvit

python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate

pip install -r requirements.txt
pip install notebook               # or use VS Code / Cursor to open the .ipynb

export GEMINI_API_KEY="your-key"   # Windows (PowerShell): $env:GEMINI_API_KEY="your-key"
jupyter notebook
```

For Experiment 11, start Ollama and pull the model first:

```bash
ollama pull llama3.2
ollama serve        # skip if the Ollama app is already running
```

### Option B: Run on Google Colab

1. Open the notebook in Colab (upload it, or open it from GitHub).
2. Run this in the first cell:

   ```python
   !pip install -q -r https://raw.githubusercontent.com/AdhimulamBhargavSaiViswanath-05/prompt-engineering-lab-vvit/main/requirements.txt
   ```

3. For Experiments 13-16, add your key in Colab **Secrets** (the key icon in the left sidebar) as `GEMINI_API_KEY`, then run:

   ```python
   import os
   from google.colab import userdata
   os.environ["GEMINI_API_KEY"] = userdata.get("GEMINI_API_KEY")
   ```

4. Experiment 11 needs a local Ollama server, which is not available on Colab by default. Run it locally, or install Ollama in the Colab runtime yourself.

A GPU runtime is optional. All experiments also work on CPU.

## Notes

- Experiments 1-10 use `distilgpt2`, a small base language model. It is not instruction-tuned, so it often repeats the prompt or ignores instructions. The experiments show *how prompt design changes the output*, so the absolute quality of the text is low.
- Experiments 5-10 use greedy decoding (`do_sample=False`), so their outputs are repeatable. Experiments 1-4 sample randomly, so outputs differ on every run.
- Experiments 13-16 use Gemini through its OpenAI-compatible endpoint, so the `openai` Python package is used with `base_url="https://generativelanguage.googleapis.com/v1beta/openai/"`.
- Experiment 15: if image generation is not available for your key (for example, free-tier quota), place an image generated from the prompt as `generated_image.png` in this folder. The captioning and alignment steps then use that file.
- Never commit your API key. Keep it in an environment variable or in Colab Secrets.

## Rebuilding the PDF record

The PDF is built from [`latex-record/main.tex`](latex-record/main.tex), which was generated from the notebook (code and outputs).

1. Create a new blank project on [Overleaf](https://www.overleaf.com).
2. Paste the contents of `latex-record/main.tex` into the project's `main.tex`.
3. Upload `latex-record/generated_image.png` to the project root (needed for Experiment 15).
4. Compile with **pdfLaTeX** (XeLaTeX also works) and download the PDF.

You can change the roll number, batch text, and starting page number at the top of `main.tex` (`\regdno`, `\batchinfo`, `\startpage`).

## Repository structure

```
.
├── PromptEngineering_IV_CSM_A_23BQ1A4201_Exps.ipynb   # All 16 experiments
├── pe_record_23BQ1A4201.pdf                           # Final lab record (college format)
├── latex-record/
│   ├── main.tex                                       # LaTeX source of the record
│   └── generated_image.png                            # Image used in the record (Experiment 15)
├── requirements.txt                                   # Python dependencies
├── generated_image.png                                # Image used by Experiment 15
├── README.md
├── LICENSE
└── .gitignore
```

## Author

Adhimulam Bhargav Sai Viswanath (Roll No. 23BQ1A4201)

## License

Released under the [MIT License](LICENSE).
