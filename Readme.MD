# Administrative Document Classification with Gemma

This project fine-tunes Gemma 2 2B to classify administrative documents and extract structured metadata from their text.

**Project category:** Classification and information extraction

**Base model:** `google/gemma-2-2b-it`

**Fine-tuning:** LoRA/SFT with Hugging Face Transformers and TRL

**Recommended environment:** Google Colab with a GPU

## Problem

Administrative documents use many formats and serve many purposes. Processing them manually can make it difficult to identify a document's type, purpose, key dates, responsible office, and other important fields consistently.

## Solution

The system accepts a document text prompt and uses a fine-tuned language model to determine:

- Document format or genre
- Administrative purpose or action
- Available extracted fields, including title or subject, date, sender, recipient, reference number, required action, deadline, and responsible person or office

For scanned or photographed documents, OCR or another text-extraction process should be applied before classification.

## Supported categories

**Document formats:** letter, memo, notice, circular, email, and article.

**Administrative purposes:** request for approval, notice of approval, appointment, recommendation, event invitation, leave request, sick leave, maternity leave, posting, query, petition, report, minutes of meeting, admission letter, and promotion letter.

An `other/unclear` label is used for unsupported or ambiguous documents. Urgency is represented separately through dates, deadlines, and time-to-event rather than as a document type.

## Dataset

The dataset combines synthetic administrative documents with authentic academic administration documents.

- Real examples must be authorized and redacted before inclusion.
- Synthetic examples add variation in wording, structure, format, and layout.
- Each record is identified by source type.
- Incomplete or unreadable fields are marked unknown rather than inferred.

See [DATASET_CARD.md](DATASET_CARD.md) for the complete data card.

## Evaluation

The notebook evaluates predictions against both document-format and administrative-purpose labels for authentic documents. It should report:

- Document-format accuracy
- Administrative-purpose accuracy
- Classification output for representative examples

Ambiguous, unsupported, or OCR-impaired examples should not be counted as successfully extracted without considering their uncertainty.

## Getting started

1. Open `Capstone.ipynb` in Google Colab.
2. Mount Google Drive if you are using the workbook paths configured in the notebook.
3. Enable a GPU runtime.
4. Install the notebook dependencies.
5. Add the synthetic and authentic document datasets to the configured data directory.
6. Run the notebook from top to bottom.

The repository also includes a fine-tuned LoRA adapter in `gemma-instruction-adapter/`.

## Export and run the full model

The adapter in `gemma-instruction-adapter/` is a LoRA adapter, not a standalone model. Use [inference_colab.ipynb](inference_colab.ipynb) to load it, merge it into the Gemma base model, and upload the full model to a Hugging Face model repository. The exporter creates a private repository by default and requires a Hugging Face token with write access.

After exporting, use [inference_huggingface_colab.ipynb](inference_huggingface_colab.ipynb) to load the full model directly from Hugging Face and run inference. A read token is needed for private repositories; public access must comply with the applicable Gemma terms.

## Load the adapter directly

For applications that prefer using the adapter without exporting a merged model:

```python
from peft import PeftModel
from transformers import AutoModelForCausalLM, AutoTokenizer

base_model_name = "google/gemma-2-2b-it"
adapter_repo = "Preciousunoka/gemma-finetuned"

tokenizer = AutoTokenizer.from_pretrained(base_model_name)
base_model = AutoModelForCausalLM.from_pretrained(
    base_model_name,
    device_map="auto",
)
model = PeftModel.from_pretrained(base_model, adapter_repo)
```

## Example adapter usage

```python
prompt = """Analyze the document below and fill in the classification metadata.

Document text:
All faculty members are requested to attend the sensitization program in the main auditorium this Thursday at 10:00 AM.

Document Format:
Administrative Purpose:
"""

inputs = tokenizer(prompt, return_tensors="pt").to(model.device)
outputs = model.generate(**inputs, max_new_tokens=200)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

## Repository files

- `Capstone.ipynb` - primary fine-tuning and evaluation notebook
- `DATASET_CARD.md` - dataset scope, labels, creation process, and limitations
- `gemma-instruction-adapter/` - trained LoRA adapter and tokenizer files
- `inference_colab.ipynb` - merge the adapter and export the full model to Hugging Face
- `inference_huggingface_colab.ipynb` - run inference using the full Hugging Face model
