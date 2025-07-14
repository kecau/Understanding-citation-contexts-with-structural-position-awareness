#  Spatial-Aware Citation Intent Dataset

This repository contains the dataset and resources used in this paper:

**"Spatial-Aware Citation Intent Classification to Mitigate Class Imbalance"** (Submitted to MIWAI 2025)

---

##  Dataset 

The dataset is derived from 818 full-text articles from the Journal of Informetrics (Vol. 7–16), retrieved via the Scopus API. It is designed for training and evaluating citation intent classification models by incorporating section-level information.

###  Dataset used in this research 

Preprocessed Dataset.csv: This CSV file is the final processed dataset used in this experiment. The structure of this dataset contains paper_id, sentence, sec_id, section_name, section_cat, intent


```csv


| Column         | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| `paper_id`     | Unique article identifier (e.g., DOI or internal ID)                    |
| `sentence`     | The citation sentence from the article                                  |
| `sec_id`       | Section ID of the citation sentence                                     |
| `section_name` | Raw section name (e.g., "2. Related Work")                              |
| `section_cat`  | Normalized section category (e.g., Introduction, Method, Results, etc.) |
| `intent`       | Citation intent label (e.g., background, uses, motivation, etc.)        |
```

Statistics
Total Instances: 26,246

### Intent Label Distribution

```json
{
  "background": 21657,
  "uses": 2684,
  "similarities": 738,
  "differences": 527,
  "motivation": 433,
  "extends": 134,
  "future_work": 61
}
```
*Note: Some minor label noise exists (e.g., misparsed section names), but no filtering was applied after preprocessing.

### Citation sentence dataset Components

### sentence.zip
Contains citation-level annotations from 818 papers. It has the following schema:

```json
{
  "sentence": "The method described by Smith et al. [3] was adopted in our study.",
  "bib_id": "bib3",
  "intent": "uses",
  "offset_start": 1985,
  "offset_end": 2023
}
```
### section.zip
Contains structural metadata for each paper. One JSON per article. It has the following schema:

```json
{
  "section_id": "sec0003",
  "title": "2. Related Work",
  "start": 1345,
  "end": 1894,
  "depth": 1
}
```
*Used to assign sec_id and derive section_cat.

### controlled_outline.json
Maps noisy or variable section names to standardized categories, It has the following schema:
```json
{
  "2.Relatedwork": "Introduction",
  "5.Experimentalsetup": "Method"
}
```
### bibid_check.json
Links in-text citation markers to external identifiers like DOIs, It has the following schema:
```json
{
  "bib3": "10.1000/xyz123",
  "bib7": "10.1016/j.artint.2020.103423"
}
```
## Data Preprocessing 

The following steps were applied to create Preprocessed Dataset.csv:

1. Extract citation sentences from *_sentence_intent.json

2. Map each citation to section metadata via section.zip

3. Normalize section names using controlled_outline.json

4. Map in-text citation IDs to external references via bibid_check.json

5. Merge all into a unified DataFrame and save as CSV
