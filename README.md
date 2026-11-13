# Jordanian PC Builder (JPCB) — Phase 2

## Project Overview

This repository holds Phase 2 of the **Jordanian PC Builder (JPCB)** project. Our goal is to train a small AI model (`gpt2`) on local PC parts and compatibility guides. We built a full training process, including Continued Pretraining (CPT) and Supervised Fine-Tuning (SFT), to make the model an expert in our specific topic.

## Team Contributions

**We all worked together** to scrape the local PC catalog data, and we discussed and made all major project decisions as a team. Here is how we broke down the rest of the tasks:

* **Saif:** Handled data preprocessing and cleaning, built the Continued Pretraining (CPT) code, made the first SFT training demo, and was the 1st grader for evaluating the model.
* **Sulieman:** Finished the Supervised Fine-Tuning (SFT) training, wrote the code to automatically upload files to GitHub and Hugging Face, and set up the repository structure.
* **Shihab:** Collected the technical articles for training, created the 500 SFT question-answer pairs, translated the 30 test questions from Arabic to English , and was the 2nd grader.
* **Yahya:** Wrote the original 30 Arabic test questions, acted as the final judge for any grading disagreements, and wrote the final Phase 2 report.

## Model Checkpoints (Private Hugging Face Repo)

To follow the Phase 2 rules and avoid overloading GitHub, we didn't save our huge `.safetensors` model files here. 

Instead, our fully trained **CPT** and **SFT** models are safely stored in a **private Hugging Face repository**.

## Repository Structure

We put everything into one easy-to-use notebook so anyone can run it:

* `PC-Builder.ipynb`: The main Jupyter Notebook with everything in one place (data loading,preprocessing,CPT, SFT, tracking metrics, and scoring the 30 questions).
* `config.yaml`: Holds our main settings for training and testing.
* `SCOREBOARD.md`: Shows the accuracy, refusal accuracy, and perplexity of our Base, CPT, and SFT models on the 30 test questions.
* `data/`: Contains our local parts catalog (`components.csv`), technical PDFs, and the `sft_pairs.jsonl` file we made.
* `tests/`: Contains our official benchmark `test_questions.csv`.
* `results/`: Holds the loss curve charts (`cpt_loss.png`, `sft_loss.png`), training history logs, and CSV files showing how each model answered.

## Setup Instructions & How to Run

Since everything is in one notebook, running our project is very simple.

1. **Environment Setup:**
   Open `PC-Builder.ipynb` in Google Colab (we recommend using a T4 GPU). The first cell automatically installs all the needed libraries:

   ```bash
   pip install "transformers>=4.50,<5" "accelerate>=1,<2" "pymupdf>=1.24" "pandas>=2" "matplotlib>=3.7" "sentencepiece>=0.2" "gitpython" "huggingface_hub"
   ```

2. **Execution:**
   Run the notebook cells from top to bottom. It will automatically:
   * Download this GitHub repo to get the `data/` and `config.yaml` files.
   * Run Continued Pretraining (CPT) and save the scores.
   * Run Supervised Fine-Tuning (SFT) so the model learns how to answer properly.
   * Test the Base, CPT, and SFT models on our `tests/test_questions.csv` file.
   * Save the results and charts in the `results/` folder.
   * Upload the light files to GitHub and the heavy model files to Hugging Face (if you have access to the required tokens).