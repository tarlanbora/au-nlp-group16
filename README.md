# Natural Language Processing Group 16

## Abstract

The project explores a typology-aware multilingual machine translation approach using the SMOL dataset, which contains professionally translated text for 221 low-resource languages along with rich metadata, including script type, language family, and Glottocodes. The goal is to investigate whether integrating such metadata directly into a multilingual model can improve translation quality for underrepresented languages. Using mBART-50 as the base architecture, we construct a “typology embedding” for each language by combining a one-hot script vector, a family/region embedding, and simple corpus-derived indicators such as average sub-tokens per word and type-token ratio. These embeddings are added to the model’s language-ID representations during fine-tuning, enabling the model to learn script- and family-sensitive patterns. Evaluation using chrF++, spBLEU, and COMET metrics will determine whether these lightweight, in-dataset typological signals enhance cross-lingual transfer and overall translation performance in low-resource settings.

## Contributions

This project introduces a typology-aware multilingual translation approach that uses only SMOL’s internal metadata - script type, language family, and basic linguistic statistics - to improve low-resource translation. The novelty lies in creating data-driven typology embeddings directly from SMOL and integrating them into the mBART-50 model’s language-ID representations. This method enables the model to adapt to each language’s structural traits without external databases, offering a reproducible way to enhance cross-lingual transfer in multilingual MT.

## Methods

The initial work on the project was a review of the SMOL dataset and its published paper to understand the construction, language coverage, and the make-up of its metadata. This review was followed by a screening of related research using SMOL, identifying preprocessing strategies that were commonly employed and current limitations in low-resource multilingual translation. This review of the academic landscape gave us intuitions on how we intend to structure and make use of SMOL’s metadata fields, such as script type from the ISO 15924 column, continent-based family indicators, and Glottocodes.

An exploratory data analysis will assist us in characterizing SMOL’s linguistic and structural diversity. Some statistics of the data such as token and sentence lengths, vocabulary size, and tokenization behavior across languages were examined. From this, we will derive two continuous features for each language: average sub-tokens per word and type-token ratio. These will later be used as measures to differentiate morphological identities.

Next, the data preprocessing step will ensure consistency and allow for seamless model integration. This step includes the cleaning and the normalization of text, applying mBART’s SentencePiece model for subword tokenization, and encoding each language with its proposed metadata fields. For each language, a typology embedding will be constructed by concatenating a one-hot encoding of script type, an embedding of family/region which will be learned, and the continuous  morphological indicators. These embeddings will be combined with mBART-50’s existing vectors, allowing for the model to consider both structural and typological information in machine translation.

Fine-tuning will be conducted on SMOL’s multilingual parallel data using mBART-50 as the base model. The training objective will mirror standard sequence-to-sequence translation fine-tuning. However, the difference will be at both encoder and decoder stages, where the introduction of typology-enhanced embeddings will serve as auxiliary inputs. Experiments will compare baseline mBART-50 performance with and without typology embeddings to observe their contribution.

In the evaluation step, metrics such as chrF++, spBLEU, and COMET will be utilised to assess translation quality across multiple low-resource directions. We will conduct both aggregate and per-language analyses to measure improvements and understand which typological factors help the model learn better connections between languages.

## Proposed timeline

## Organization within the team


# Appendix

## Repo organization

## Questions for the TA


