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

I recommend config B with k=5. Its annual cost is $15,755.12, which is within the $18,400 budget. It achieves 60.00% overall recall (95% Wilson interval: 47.37%–71.43%). Config A has higher recall (66.67%) but is too expensive and has a budget of $27,918.82, which is not in the budget. Intervals for B with k=3 and B with k=5 overlap, so it is not possible to conclude that there is a statistically significant difference between their recall using intervals. For B with k=5, recall is 60% for EN, 65% for RU, and 55% for KK. Users of the language Kazakh get the worst service, and it would be better to improve service for them.

### 1e. Next measurement (at most 60 words)

Conduct a more comprehensive retrieval assessment based on more EN, RU and KK questions, particularly Kazakh questions. Calculate a 95% Wilson interval for each language and each candidate and report the results of @recall. This would indicate whether the observed language differences endure, and whether the increased expense of config A is warranted by obtaining reliable retrieval improvements.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H |86.11%	|KK, clean |76.67%	|59.07% |88.21% |$20,190.49 |
| L |80.56%	|RU, degraded |70.00% |52.12%	|83.34%	|$21,600.00 |

Personal-data pages per year sent abroad under H:501875

### 2b. Memo (at most 300 words)

The overall extraction accuracy for Pipeline H is higher than Pipeline L (86.11% vs. 80.56%) and the estimated annual cost is lower ($20,190.49 vs. $21,600). In a data centre abroad, however, Pipeline H processes images of invoices, which raises a privacy concern: 25% of the pages include personal names and ИИН. This is about 501,875 pages per year exported.

The pipeline is weak with respect to Kazakh clean invoices too, with an accuracy of 76.67%. The most challenging sets are the degraded Russian invoices, with only 70.00% accuracy. These results demonstrate the need for more testing of both pipelines in different languages and scan conditions.

If storing personal information within the borders of Kazakhstan is of primary importance, then Pipeline L is recommended. It might be $1,409.51 more expensive per year, but it results in greater inaccuracy, it's local processing which means less risk of sending sensitive information to other countries. Pipeline H could be chosen if legal and suitable data protection measures are put in place and cross-border processing is allowed. Prior to deployment I tested both pipelines on a bigger set of data, optimized the performance of their weakest cells and ensure that the invoice number, dates, seller ID and totals are being extracted correctly.

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
