# Political Bias Detection in News Headlines

**CIS 5190 Final Project** | Rushil Patel, Sarthak Jain, Tanner Shah

## Overview
Can you predict the political bias of a news article from its headline alone? This project explores that question by training classifiers to distinguish Fox News from NBC headlines, then fine-tuning on CNN, NPR, and DailyWire to detect partisan framing more broadly.

## Models Evaluated
- Naive Bayes & Logistic Regression (baselines)
- Bi-LSTM
- ELECTRA Small (Lightweight Transformer)
- DistilBERT ✅ *(best performer — ~98% accuracy on Fox/NBC)*

## Results
| Task | Model | Accuracy |
|---|---|---|
| Fox vs. NBC | DistilBERT | ~98% |
| DailyWire vs. CNN/NPR | Fine-tuned DistilBERT | >95% |
| DailyWire vs. NPR vs. CNN (3-class) | Fine-tuned DistilBERT | Outperformed GPT-4 baseline |

The model confidently distinguishes DailyWire from left-leaning outlets, but tends to conflate NPR and CNN — suggesting those outlets share more stylistic overlap than either does with DailyWire.

## Exploratory Component: Political Bias Detection
We froze most of DistilBERT's layers and fine-tuned the classifier head on DailyWire, CNN, and NPR headlines to test whether partisan framing is detectable at the headline level. We also compared against a ChatGPT-based pseudo-SOTA baseline (inspired by Penn's [PennMAP Media Bias Detector](https://pennmap.org/)) — our fine-tuned model outperformed it.

## Data Collection
Headlines were scraped via `sitemap.xml` using an async pipeline with randomized headers and jitter to avoid rate limiting. Cleaning included deduplication, removal of non-article URLs, and normalization. Final datasets:
- Fox/NBC: ~1M URLs each
- CNN/NPR/DailyWire: 100–400 articles each (upsampled for balance)

## Repo Structure
```
├── CIS_5190_FINAL_Project.ipynb   # Data cleaning, training, and evaluation
├── report.pdf                      # Full project report
```

## Key Takeaway
Headline-level word choice and framing appear sufficient to identify news source and detect partisan lean — particularly for strongly partisan outlets. Performance degrades when distinguishing outlets with more similar editorial styles (e.g., NPR vs. CNN).
