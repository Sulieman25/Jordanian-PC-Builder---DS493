# Phase 2 Scoreboard: Jordanian PC Builder (JPCB)

## Evaluation Setup

We evaluated the models using a test set of 30 official English questions. Saif and Shihab independently graded the responses and agreed on all 90 scores during their first pass, giving us a 100% inter-rater agreement. 

During the evaluation, we tracked overall accuracy (scored out of 30), refusal accuracy (how well the model handled the 18 unanswerable questions), average latency in seconds, and perplexity.

## Model Performance

| Model Version | Held-out PPL | Accuracy | Refusal Accuracy | Avg Latency (s) | Notes | 
| ----- | ----- | ----- | ----- | ----- | ----- | 
| **GPT-2 Base** | 51.0544 | 1.67% | 0% | 0.8229 | Original pre-trained weights. | 
| **GPT-2 + CPT** | 10.5629 | 1.67% | 0% | 0.8230 | 79.3% decrease in perplexity. | 
| **GPT-2 + SFT** | 2.8193 | 0.00% | 0% | 0.3224 | Used answer-only loss. | 

## SFT Error Breakdown

The SFT model didn't score any points on the final test set. Here is a quick breakdown of where it struggled across the 30 responses:

* **Refusal Errors (19):** The model failed to reject out-of-scope requests or unavailable items.
* **Hallucinations (6):** The model invented prices or product specifications.
* **Missing Knowledge (3):** The model simply lacked the required information.
* **Format Failures (2):** The model didn't output the text in the expected format.
* **Test-case Problems (0):** No issues with the test questions themselves.

## Training Settings

### Continued Pretraining (CPT)

We started with the base GPT-2 small model, which has roughly 124 million parameters. For the training data, we used 2,245 clean catalogue rows alongside 14 PC hardware PDF guides. This was split into 176,091 training tokens and 24,656 held-out tokens, which comes out to about a 90/10 document split. 

We trained the model for 10 epochs using a learning rate of 5e-5, a block size of 512, and a batch size of 2 with 4 gradient accumulation steps. We ended up selecting the best validation checkpoint, which occurred around epoch 9.

### Supervised Fine-Tuning (SFT)

For the fine-tuning phase, we used a dataset of 500 Q&A pairs, split into 450 for training and 50 for validation. 

We ran this for 10 epochs with a learning rate of 3e-5 and a max length of 384, but we opted to use the checkpoint from epoch 8 since it had the best validation loss and helped us avoid mild overfitting. The format relied on standard chat markers (`<|user|>`, `<|assistant|>`, `<|end|>`), and we trained it using answer-only loss by masking the question tokens.