# Midterm 1: answers

GitHub username: AdamNarxoz
Session code: MAPLE-66
Fingerprint: 943731e2

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

(About half a page: which assistants or tools you used and for what. If you used none, say so.)

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A |0.4000 (40.00%) |0.5500 (55.00%) |0.6667 (66.67%) |0.5056 |
| B |0.1667 (16.67%) |0.3167 (31.67%) |0.6000 (60.00%) |0.2913 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | | | |
| A, k=5 | | | |
| B, k=3 | | | |
| B, k=5 | | | |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | |
| A, k=5 | |
| B, k=3 | |
| B, k=5 | |

### 1d. Decision (at most 150 words)

Chosen candidate:

(Decision, using your intervals and the budget. Give the recall of your choice for EN, RU and KK separately.)

### 1e. Next measurement (at most 60 words)

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | | | | | | |
| L | | | | | | |

Personal-data pages per year sent abroad under H:

### 2b. Memo (at most 300 words)

## Part 3: Explain your own numbers

### Q1

Number: 1.67

Why Kazakh's token premium is about 1.67 times its byte premium. UTF-8 encoding explains part of the difference, but the tokenizer adds substantial overhead.:

### Q2

Number:434 e5 tokens

Why The answer sentence starts within the first 512 e5 tokens, so it can fit in the embedding model's input window. No truncation error is expected for this sentence, although later text may be truncated.:

### Q3

Number: 84.6%

Why  Lower temperature makes the probability distribution sharper, increasing Monday's probability without changing the token ranking. All five tokens remain possible because temperature scaling alone does not remove tokens.:

### Q4

Number: 22.7%

Why I excluded multiplication and arithmetic sequences because they can be solved using learned rules rather than memorized facts. A system prompt saying “never invent a fact” cannot change this bound; it may encourage abstention instead.:

### Q5

Number: 16.10

Why The model uses 16.10 times Chinchilla's compute-optimal tokens-per-parameter ratio, so it is over-trained by this criterion. Chinchilla showed that GPT-3-sized models were under-trained and that smaller models trained on more tokens could perform better at comparable compute.:
