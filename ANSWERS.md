# Midterm 1: answers

GitHub username: AdamNarxoz
Session code: MAPLE-66
Fingerprint: 943731e2

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I let ChatGPT help me understand the requirements of the assignment, verify formulas, compute retrieval metrics and confidence intervals, and approximate annual cost of the model. I used Python and pandas to make sense of the CSV files generated and to check my work in Google Colab. I also discussed how accurate one needs to be in order to keep the computations low, the privacy of the data, and behavior of the model. The numerical results are computed for my synthetic data and the parameter given in the assignment. I reviewed the figures and made the final decisions on the figures.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A |0.4000 |0.5500 |0.6667 |0.5056 |
| B |0.1667 |0.3167 |0.6000 |0.2913 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 |0.5500 |0.4249 |0.6691 |
| A, k=5 |0.6667 |0.5406 |0.7727 |
| B, k=3 |0.3167 |0.2131 |0.4423 |
| B, k=5 |0.6000 |0.4737 |0.7143 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 |$20,989.97 |
| A, k=5 |$27,918.82 |
| B, k=3 |$13,691.75 |
| B, k=5 |$15,755.12 |

### 1d. Decision (at most 150 words)

Chosen candidate:B, k=5

I would like to suggest using config B (k=5). Its annual cost is $15,755.12, which is within the $18,400 budget. It achieves 60.00% recall (95% Wilson interval: 47.37%–71.43%). While Config A provides a higher recall (66.67%), it is more expensive ($27,918.82) than the budget. By itself, the overlap of the Wilson intervals for the two values of k, k=3 and k=5, does not necessarily indicate that there is no statistically significant difference. For config B with k=5, recall is 60% for EN, 65% for RU, and 55% for KK. For the three languages, the recall is lowest in Kazakh, so strengthening the retrieval of the language should be prioritized.

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

The locally developed vision-language model pipeline L is recommended.

Pipeline H has an overall accuracy of 87.22% whereas L's accuracy is 86.67%. But when it comes to degraded Russian invoices, H is less accurate than L (66.67% accuracy vs. 80.00%). These 95% Wilson interval do not exclude 0, so more testing would be required to determine if the difference is statistically significant.

Pipeline H costs approximately $20,205.64 per year, while L costs $21,600.00. The extra annual expense of L is about $1,394.36. This is a small extra charge for maintaining the images of the invoices in our own system, and providing better observed performance on bad Russian scans.

The privacy issue is noteworthy. Under H. Pipeline L, about 501,875 personal-data pages would be sent abroad each year at 5,500 pages per day, where approximately 25% have personal information, such as the name of the individual and a code or identifier for the person's IIN.

The test would be completed on a larger data set (including a representative degraded scan set of all three languages) before the pipelines were deployed. I would also validate the number of errors I would see in the field—particularly in invoice totals, dates, and seller ID—and confirm L's actual throughput and operating costs. Low-confidence extractions and high impact errors should be monitored for production and/or reviewed by humans.

So I've decided to go with L, since there's a trade-off with performance on the least computer-heavy test condition, but the control of personal data is on my side, and the extra annual cost is not a significant amount.

## Part 3: Explain your own numbers

### Q1

Number: 1.85

Why:  Kazakh has 1.96 times as many UTF-8 bytes as English but 3.62 times as many tokens. The normalized ratio of 1.85 indicates that the tokenizer adds a substantial premium beyond the difference explained by byte length.

### Q2

Number: 552   

Why:  The sentence starts beyond the 512-token limit. A question matching only that sentence cannot retrieve the truncated chunk through that evidence, and truncation may occur silently without an obvious error.

### Q3

Number: 39.5%

Why: Increasing the temperature preserves the ranking of tokens but makes their probabilities more uniform. All listed tokens remain possible to sample because softmax assigns each a positive probability.

### Q4

Number: 24

Why: The public leaderboard prefers X because it has 76 correct answers versus Y's 74. The penalized score favors Y because it makes fewer mistakes and can abstain without penalty, illustrating how evaluation rules shape model behavior.

### Q5

Number: 15.00
Why: The model uses 300 tokens per parameter, compared with Chinchilla's 20. By this ratio, it is over-trained relative to the stated compute-optimal benchmark. Chinchilla argued that models such as GPT-3 were under-trained and could benefit from more training data.
