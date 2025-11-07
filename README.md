# Natural Language Processing Group 16

## Abstract

The project explores a typology-aware multilingual machine translation approach using the SMOL dataset, which contains professionally translated text for 221 low-resource languages along with rich metadata, including script type, language family, and Glottocodes. The goal is to investigate whether integrating such metadata directly into a multilingual model can improve translation quality for underrepresented languages. Using mBART-50 as the base architecture, we construct a “typology embedding” for each language by combining a one-hot script vector, a family/region embedding, and simple corpus-derived indicators such as average sub-tokens per word and type-token ratio. These embeddings are added to the model’s language-ID representations during fine-tuning, enabling the model to learn script- and family-sensitive patterns. Evaluation using chrF++, spBLEU, and COMET metrics will determine whether these lightweight, in-dataset typological signals enhance cross-lingual transfer and overall translation performance in low-resource settings.

## Contributions

This project introduces a typology-aware multilingual translation approach that uses only SMOL’s internal metadata - script type, language family, and basic linguistic statistics - to improve low-resource translation. The novelty lies in creating data-driven typology embeddings directly from SMOL and integrating them into the mBART-50 model’s language-ID representations. This method enables the model to adapt to each language’s structural traits without external databases, offering a reproducible way to enhance cross-lingual transfer in multilingual MT.

## Methods

The initial work on the project was a review of the SMOL dataset and its published paper to understand the construction, language coverage, and the make-up of its metadata. This review was followed by a screening of related research using SMOL, identifying preprocessing strategies that were commonly employed and current limitations in low-resource multilingual translation. This review of the academic landscape gave us intuitions on how we intend to structure and make use of SMOL’s metadata fields, such as script type from the ISO 15924 column, continent-based family indicators, and Glottocodes.

An exploratory data analysis will assist us in characterizing SMOL’s linguistic and structural diversity. Some statistics of the data such as token and sentence lengths, vocabulary size, and tokenization behavior across languages were examined. From this, we will derive two continuous features for each language: average sub-tokens per word and type-token ratio. These will later be used as measures to differentiate morphological identities.

Next, the data preprocessing step will ensure consistency and allow for seamless model integration. This step includes the cleaning and the normalization of text, applying mBART’s SentencePiece model for subword tokenization, and encoding each language with its proposed metadata fields. For each language, a typology embedding will be constructed by concatenating a one-hot encoding of script type, an embedding of family/region which will be learned, and the continuous morphological indicators. These embeddings will be combined with mBART-50’s existing vectors, allowing for the model to consider both structural and typological information in machine translation.

Fine-tuning will be conducted on SMOL’s multilingual parallel data using mBART-50 as the base model. The training objective will mirror standard sequence-to-sequence translation fine-tuning. However, the difference will be at both encoder and decoder stages, where the introduction of typology-enhanced embeddings will serve as auxiliary inputs. Experiments will compare baseline mBART-50 performance with and without typology embeddings to observe their contribution.

In the evaluation step, metrics such as chrF++, spBLEU, and COMET will be utilised to assess translation quality across multiple low-resource directions. We will conduct both aggregate and per-language analyses to measure improvements and understand which typological factors help the model learn better connections between languages.

## Proposed timeline

### 7-21 November

We estimate that about two weeks will be spent finetuning the model in its different configurations. By creating variants of our finetuned model with different configurations of typology-enhanced embeddings we will be able to investigate what elements may be beneficial and to what degree regarding machine translation.

Some of this time will be spent writing the code to retrieve and organize the internal metadata so that is can be used to train the model.

### 21 - 05 December

We estimate the following two weeks will be spent running evaluation metrics across the model variations. This also includes keeping track of the changes made by typology embeddings in aggregate or various languages, categorizing and tabularizing them to ease work in the report stage.

We may begin the report if we complete our evaluations early. Should we find that our models do not show any improvements (Or show degradation in translation quality), we may also spend this time figuring out how we can change our approach to improve our results from baseline.

### 05 - 19 December

Finally we will spend the last two weeks analyzing our results and writing the report. We will focus on making a clear and concise report that displays the most significant results and observations from our evaluation data. We expect to have enough time to properly explore not just our results but where our findings could be taken in future research.

## Organization within the team

We want to ensure that every member of the team has a hand in most areas, so that we get the most out of the course. However, to ensure that everything goes according to our rough timeline, we each hold responsibility to certain areas of work.

* Preparing the metadeta and generating per-language features:
* Fine tuning the model:
* Running evaluation metrics with the fine-tuned models:
* Aggregating and tabularizing results:
* Analysis and discussion of results:
* Report, repository, misc: 


# Appendix

## Repo organization

This repository is organised to make it easy to find the data, metadata, preprocessing artifacts, and experiment notebooks used in the project. Below is a short guide to the top-level layout and conventions.

- `main.ipynb` — notebook containing project implementation
- `README.md` — this file (project overview and usage notes).
- `smol/` — dataset folder, added to .gitignore to prevent upload of the dataset. To download go to https://huggingface.co/datasets/google/smol
  - `README.md` — notes specific to the contents of the `smol/` folder and dataset usage.
  - `smoldoc-factuality-ratings.json` — factuality ratings and related metadata used in analysis.
  - `gatitos/` — directory with parallel data files (JSON Lines) split by language pairs.
  - `smoldoc/` — directory with parallel data files (JSON Lines) split by language pairs.
  - `smolsent/` — directory with parallel data files (JSON Lines) split by language pairs.
    - Files are named like `en_es.jsonl`, `ace_en.jsonl`, or `en_ace.jsonl`. Each file contains one JSON object per line representing a parallel sentence pair.
    - Convention: filenames use the pattern `<lang1>_<lang2>.jsonl`. Check the file name to infer the source/target order used in that file.

## Questions for the TA
