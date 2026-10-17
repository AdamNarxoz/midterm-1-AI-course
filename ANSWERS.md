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
| A |0.4000 |0.5500 |0.6667 |0.5056 |
| B |0.1667 |0.3167 |0.6000 |0.2913 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 |33/60	|55.00% |42.49%–66.91% |
| A, k=5 |40/60	|66.67%	|54.06%–77.27% |
| B, k=3 |19/60	|31.67%	|21.31%–44.23% |
| B, k=5 |36/60	|60.00%	|47.37%–71.43% |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 |$20,989.97 |
| A, k=5 |$27,918.82 |
| B, k=3 |$13,691.75 |
| B, k=5 |$15,755.12 |

### 1d. Decision (at most 150 words)

Chosen candidate:B, k=5

I recommend config B with k=5. Its annual cost is $15,755.12, which is within the $18,400 budget. It achieves 60.00% overall recall (95% Wilson interval: 47.37%–71.43%). Config A with k=5 has higher recall (66.67%), but costs $27,918.82 and exceeds the budget. The intervals for B with k=3 and B with k=5 overlap, so we cannot conclude that their recall differs statistically using these intervals. For B with k=5, recall is 60% for EN, 65% for RU, and 55% for KK. Kazakh-speaking users are served worst, so improving retrieval for Kazakh should be a priority.

### 1e. Next measurement (at most 60 words)

Run a larger, stratified retrieval evaluation with more EN, RU, and KK questions, especially Kazakh questions. Measure recall@5 and compute 95% Wilson intervals for each language and candidate. This would show whether the observed language differences persist and whether the extra cost of config A is justified by a reliable improvement in retrieval quality.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H |86.11%	|KK, clean |76.67%	|59.07% |88.21% |$20,190.49 |
| L |80.56%	|RU, degraded |70.00% |52.12%	|83.34%	|$21,600.00 |

Personal-data pages per year sent abroad under H:501875

### 2b. Memo (at most 300 words)

Pipeline H achieves higher overall extraction accuracy (86.11%) than Pipeline L (80.56%) and has a lower estimated annual cost of $20,190.49 compared with $21,600. However, Pipeline H processes invoice images in a data centre abroad, creating a privacy concern because 25% of pages contain personal names and ИИН. This represents approximately 501,875 pages per year sent abroad.

Pipeline H also has a weakness in Kazakh clean invoices, where accuracy is only 76.67%. Pipeline L performs worst on degraded Russian invoices, with 70.00% accuracy. These results show that both pipelines need further testing across languages and scan conditions.

I recommend Pipeline L if keeping personal data within Kazakhstan is the priority. Although it costs $1,409.51 more per year and has lower overall accuracy, local processing reduces the risk associated with transferring sensitive information abroad. Pipeline H could be selected if cross-border processing is legally permitted and appropriate data protection safeguards are implemented. Before deployment, I would test both pipelines on a larger dataset, improve performance on their weakest cells, and verify extraction accuracy for invoice numbers, dates, seller IDs, and totals.

## Part 3: Explain your own numbers

**1. Language tokenisation**

Kazakh's token-to-byte ratio is higher than the ratio for English. Some of the difference in the number of bytes comes from UTF-8 encoding and the additional overhead from the tokenizer, which yields more tokens.

**2. Embedding model truncation**

The token 434 belongs to the embedding model, which has a maximum of 512 tokens. The sentence should, therefore, be less than the length of the input window, but later sections may be cut off.

**3. Temperature and probability**

The probability of the top token, “Monday,” is estimated at 84.6%. A smaller temperature causes the distribution of probabilities to be more peaked around the top-scoring token, but it does not affect what tokens are on top or eliminate alternatives.

**4. Memorisation and factual errors**

The lower bound estimated when asking about an account number is 22.7%, and for the public figures' birthdays, it is 25.3%. Multiplication and arithmetic sequences are not included as they are able to be solved by rules. This limitation cannot be removed by a prompt that tells a model not to generate facts: the model may choose to refrain from doing so.

**5. Tokens per parameter**

The model contains roughly 322 tokens/training parameter, whereas the reference ratio used by the Chinchilla model is 20. This is roughly 16.10 times as many training tokens per parameter under this comparison.

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
